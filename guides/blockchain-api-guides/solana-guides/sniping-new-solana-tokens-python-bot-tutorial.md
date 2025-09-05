---
description: >-
  Developer guide: build a Python bot to detect new Solana SPL mints and
  automate swaps via Jupiter
---

# Sniping new Solana tokens – Python bot tutorial

Bots are commonly used on Solana to snipe new tokens and trade on DEXes. In this example, we’ll build a Python bot that:

* Detects newly minted SPL tokens quickly (via [pump.fun](https://getblock.io/marketplace/projects/pump-fun/) or a similar program)&#x20;
* Extracts the mint address and related accounts
* Atomically swaps SOL → token using a swap aggregator (Jupiter) while applying priority fees and defensive checks

***

### Prerequisites

* Python 3.8, or higher (3.10 recommended)
* Python packages:

```bash
pip install "solana>=0.36.0" "solders>=0.26.0" websockets aiohttp requests
```

{% hint style="info" %}
Use the latest compatible versions of **solders** and **solana-py** as APIs change frequently.
{% endhint %}

* Accounts & keys:
  * GetBlock account + access token (JSON-RPC and WSS endpoints)
  * For devnet testing: set up a dev wallet and fund it with SOL using `solana airdrop`

{% hint style="info" %}
Always test on devnet (or a local validator) before using mainnet.
{% endhint %}

#### **Getting an RPC access token**

To call Solana nodes through GetBlock, you need an RPC endpoint with an access token. To get a token:&#x20;

1. Create an account on GetBlock.io&#x20;
2. Create a Solana endpoint from your dashboard&#x20;
3. Copy the generated access token/endpoint&#x20;

The access token is embedded in the endpoint URL. See examples below or visit our [access-token docs](../../../getting-started/authentication-with-access-tokens.md) for details.

#### Endpoints (example formats)

* **WebSocket (PubSub)**: `wss://go.getblock.io/<ACCESS_TOKEN>`
* **HTTP JSON-RPC**: `https://go.getblock.io/<ACCESS_TOKEN>`

Use environment variables or a secrets manager for these strings (do not hardcode them in source).

#### Plans and rate limits&#x20;

GetBlock provides tiered [plans](../../../getting-started/plans-and-limits/) with documented requests and CU limits. Choose a plan that fits your bot’s expected RPS and compute usage. For production, use a paid plan or dedicated node to avoid shared-node rate limits.

***

### Step 1: Environment setup

Install Python 3.8 (or higher). In your terminal, create a virtual environment and install the required packages using:

```bash
python -m venv solana-bot
source solana-bot/bin/activate
pip install "solana>=0.36.0" "solders>=0.26.0" websockets aiohttp requests
```

Store secrets in environment variables or a secure vault. Never hardcode private keys or GetBlock tokens in code.

_**Example env vars**:_

```bash
export GETBLOCK_RPC="https://go.getblock.io/<ACCESS_TOKEN>"
export GETBLOCK_WS="wss://go.getblock.io/<ACCESS_TOKEN>"
export BOT_KEYFILE="/path/to/encrypted/key.json"   # or use a signer service / HSM
```

***

### Step 2: Connect to GetBlock RPC&#x20;

Establish low-latency RPC and subscription streams to detect new mints in real time. Use async clients to avoid blocking. We show an async watcher that reconnects, resubscribes, deduplicates, and pushes work to a buy worker.

{% hint style="success" %}
Use [`logsSubscribe`](../../../api-reference/solana-sol/logssubscribe-solana.md) with the `mentions` filter on a target program ID. It’s faster and uses less bandwidth than subscribing to full blocks or transactions. For full transaction payloads you can also use [`blockSubscribe`](../../../api-reference/solana-sol/blocksubscribe-solana.md)/fetch [`getTransaction`](../../../api-reference/solana-sol/gettransaction-solana.md) for details. `logsSubscribe` is appropriate to detect the create instruction as quickly as possible.
{% endhint %}

_**Sketch** (adapt imports and helpers to your library versions):_

```python
# watcher.py 
import asyncio
import base64
import json
import time
from solders.pubkey import Pubkey
from solana.rpc.async_api import AsyncClient
from solana.rpc.websocket_api import connect   # solana-py websocket helper
from typing import Set

RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>"
WS_URL = "wss://go.getblock.io/<ACCESS_TOKEN>"
PUMP_PROGRAM_ID = Pubkey.from_string("6EF8rrecthR5Dkzon8Nwu78hRvfCKubJ14M5uBEwF6P")

seen_signatures: Set[str] = set()

async def handle_new_mint(tx_sig: str, client: AsyncClient):
    # push to a worker queue (async), do parsing & buying there
    print("Worker: would process", tx_sig)
    # Worker should call get_transaction/get_parsed to decode actual mint/address

async def watch_logs():
    backoff = 1
    while True:
        try:
            async with connect(WS_URL) as ws:
                # Subscribe for logs that "mention" the program id
                await ws.logs_subscribe({"mentions":[str(PUMP_PROGRAM_ID)], "commitment":"confirmed"})
                # Wait for sub confirmation
                sub_conf = await ws.recv()
                print("Subscribed:", sub_conf)
                while True:
                    msg = await ws.recv()
                    # msg is a parsed JSON-RPC; access logs
                    if not getattr(msg, "result", None):
                        continue
                    value = msg.result.value
                    logs = value.logs or []
                    sig = value.signature
                    # dedupe using signature
                    if sig in seen_signatures:
                        continue
                    # naive detection — look for create instruction line or "Program data:" payload
                    if any("Instruction: Create" in line or line.startswith("Program data:") for line in logs):
                        seen_signatures.add(sig)
                        # schedule worker to parse and potentially buy
                        asyncio.create_task(handle_new_mint(sig, AsyncClient(RPC_URL)))
        except Exception as e:
            print("WS error:", e)
            await asyncio.sleep(backoff)
            backoff = min(backoff*2, 30)  # simple exponential backoff

```

**Notes**:

* Use a persistent `seen_signatures` store (Redis) in production to avoid duplicate buys across restarts.
* Use `blockSubscribe` + `getTransaction(..., encoding='jsonParsed')` as fallback to get structured instruction/account info if logs are not enough. `get_parsed_transaction` equivalent is available in clients (or call HTTP RPC directly).&#x20;

***

### Step 3: Decode/parse the mint

Fetch the full transaction (jsonParsed) and inspect instructions and innerInstructions to locate the Create/InitializeMint flow:

1. Get the full transaction using the signature: `await client.get_transaction(sig, encoding="jsonParsed")` (or `get_transaction` / `getTransaction` depending on the client).&#x20;
2. Inspect `meta.innerInstructions` and `transaction.message.instructions` for the `Create` or `InitializeMint` instruction and the new mint pubkey.
3. If logs include `Program data: <base64>`, decode the base64 and parse according to that program’s schema. Example:&#x20;

```python
resp = await client.get_transaction(sig, encoding="json")
tx = resp.value
# inspect tx.transaction.message.instructions or tx.meta.logMessages
```

4. If logs don't include the mint, use `get_signatures_for_address` on the program or watch [`accountSubscribe`](../../../api-reference/solana-sol/accountsubscribe-solana.md) for newly-created accounts (less direct).

{% hint style="info" %}
**Important**: Detect the SPL mint pubkey (the token mint account). For many token-creation flows the mint is an account created by the **`SystemProgram::CreateAccount`** followed by **`spl_token::initialize_mint`**. Look for **`initializeMint`** or **`InitializeMint`** in parsed instructions.
{% endhint %}

***

### Step 4: Preflight: compute state and get a swap quote (Jupiter)

Before building a swap transaction:

* Compute the buyer’s ATA (associated token account) for the new mint:

```python
from spl.token.instructions import get_associated_token_address
ata = get_associated_token_address(owner_pubkey, mint_pubkey)
```

* If ATA doesn't exist, you must include an instruction to create it or use a swap program that will create it atomically.
* Get a swap route/quote from Jupiter’s Quote API (v6). Use the quote endpoint to compute the route and expected output for SOL → token swap, then call the Swap API or build swap instructions returned by Jupiter. [Jupiter docs](https://dev.jup.ag/docs/api/swap-api/quote) show how to request swap instructions.

_**Example**_:

```python
import requests
quote = requests.get("https://quote-api.jup.ag/v1/quote",
                     params={"inputMint":"SOL","outputMint": str(mint_pubkey), "amount": amount_in_lamports})
route = quote.json()
# Pick route[0], then call swap-instructions endpoint to get the actual instructions or to get raw swap instructions
```

Jupiter returns prebuilt swap instructions and multi-DEX routes, which are harder to craft manually and safer for unknown token pair liquidity.

***

### Step 5: Build the transaction (versioned tx + ComputeBudget)

Construct a versioned transaction with priority fees to maximize execution chance. Key points:

* Use [`getLatestBlockhash`](../../../api-reference/solana-sol/sol_getlatestblockhash.md) (they replaced/deprecated old `get_recent_blockhash`) and create a **versioned transaction** where possible to support address lookup tables and versioned messages.
* Add ComputeBudget instructions (set compute limit + set compute unit price) **as the first instructions** in the message to request priority fees. Use `solders` compute\_budget helpers to create these instructions.

_**Example**_:

```python

from solders.compute_budget import set_compute_unit_limit, set_compute_unit_price
from solders.keypair import Keypair
from solders.message import MessageV0, to_bytes_versioned
from solders.transaction import VersionedTransaction
# ... plus imports for the swap instructions from Jupiter (see their "swap-instructions" endpoint)

# Build instructions list:
instructions = []
instructions.append(set_compute_unit_limit(400_000))      # request high CU limit (example)
instructions.append(set_compute_unit_price(20_000))      # micro-lamports priority fee
# optionally: instruction to create ATA if necessary
# then: add the swap instructions returned by Jupiter
message_v0 = MessageV0.new_with_blockhash_and_instructions(payer_pubkey, recent_blockhash, instructions, ...)
tx = VersionedTransaction(message_v0, [payer_signer])
# sign and send

```

Call [`simulateTransaction`](../../../api-reference/solana-sol/simulatetransaction-solana.md) (or `client.simulate_transaction`) to check likely failure and compute unit consumption before actually sending. If simulation shows insufficient compute or other failures, adjust compute budget / swap route / slippage and retry.

***

### Step 6: Sign, submit, confirm, and retry

Sign the transaction, submit, and confirm the outcome:&#x20;

* Use secure signing: For dev, use encrypted local keyfiles. In production, use a signing service.
* Submit via RPC with a versioned transaction string or `send_raw_transaction`.
* Use `getLatestBlockhash()` for `last_valid_block_height` and blockhash.&#x20;
* If a blockhash is too old, rebuild with a fresh blockhash and resend.
* Use `simulate` then `send_raw_transaction`, then `confirm_transaction` or monitor via websocket [`getSignatureStatuses`](../../../api-reference/solana-sol/getsignaturestatuses-solana.md).
* Add retry/rebroadcast logic and limit total retries to avoid excessive fees.

***

### Worker flow (summary)

1. Receive `tx_sig` from watcher.
2. Call RPC `get_transaction(tx_sig, encoding="jsonParsed")` to extract the new mint.
3. Verify mint is not a scam token (optional heuristics: total supply, decimals, name patterns, frequently blacklisted mints).
4. Compute buyer ATA, check if it exists.
5. Query Jupiter for a quote/route for a SOL → new\_mint swap. Pick a route that fits your max slippage.
6. Build a versioned transaction with:
   1. ComputeBudget priority instructions (first)
   2. Create ATA instruction (if needed)
   3. Jupiter swap instructions
7. Simulate a transaction. If OK, sign and send. Confirm and log metrics.
8. If a transaction fails due to `ComputeBudget` limits, try increasing the compute limit (careful with cost). If it fails due to poor liquidity/slippage, abort.

***

### Production reliability recommendations

* **Reconnect and re-subscribe** with exponential backoff. Keep subscription state and rehydrate `seen_signatures` from a durable store (Redis).
* **Multi-RPC redundancy**: rotate between providers (GetBlock primary, fall back to public RPC / your own node). Detect 429s and switch.
* **Health checks & metrics**: latency (ws → detection → worker start), success rates, error counts.
* **Graceful rate-limiting**: implement per-endpoint throttling and backoff if you see rate-limit errors.

### Security and key handling

* Never commit private keys or access tokens to version control.
* Prefer a signing service/HSM in production. For local development, store private keys encrypted and load them from an environment variable or encrypted keyfile.
* Protect the GetBlock access token and rotate it if compromised.

***

This is a concise, practical guide for building a service that watches the Solana blockchain for newly created tokens and attempts automated swaps. It explains the overall workflow: detecting a new token, checking its details, getting a swap quote from Jupiter, and submitting the transaction.

{% hint style="warning" %}
Responsible use: Token-sniping strategies can violate DEX/platform terms and may be considered exploitative. Follow applicable laws and platform rules.
{% endhint %}
