---
description: >-
  Analyze SPL token-holder concentration using Solana RPC API. Practical
  examples for fetching top token accounts and plotting distribution statistics
---

# Analyzing SPL token distribution (Python)

On-chain analytics are a powerful way to understand token distribution and holder concentration. This tutorial uses Solana RPC methods (getTokenLargestAccounts, getAccountInfo, getTokenSupply) to enumerate large token accounts for an SPL mint and visualize results with Python.

***

### Quick notes before you start

* [`getTokenLargestAccounts`](../../../api-reference/solana-sol/gettokenlargestaccounts-solana.md) returns **up to 20 largest accounts** for a mint. To analyze more, fetch token accounts and aggregate them (see “Beyond the top-20” below).&#x20;
* The RPC responses include `amount`, `decimals`, and `uiAmount`/`uiAmountString`. Compute UI amounts yourself from decimals for robustness.
* Heavy queries can hit rate limits. Use GetBlock’s dedicated Solana node or an indexer for production analytics.
* Test every script and query on devnet before running them against mainnet.

***

### Prerequisites

* Python 3.8+&#x20;
* Create a virtual environment and install packages:

```bash
python -m venv solana-analytics
source solana-analytics/bin/activate
pip install solana solders matplotlib pandas requests

```

* Get a Solana RPC endpoint from GetBlock for HTTP JSON-RPC connections.

#### **How to obtain a Solana endpoint:**

1. Create an account on GetBlock.io.
2. In the dashboard, scroll down to “My endpoints” and get a Solana JSON-RPC access token.
3. Copy the generated endpoint (the access token is embedded in the URL). Store it in an environment variable.&#x20;

_Example_:

```bash
export GETBLOCK_RPC="https://go.getblock.io/<ACCESS_TOKEN>"
```

Treat the [access token](../../../getting-started/authentication-with-access-tokens.md) like a password. Do not commit it to source control or expose it in client-side code. Rotate or revoke tokens from the GetBlock dashboard if they are compromised.

***

### Step 1: Solana RPC client setup

Configure a reusable client that reads RPC endpoints from environment variables. Create a small `client_setup.py` that other scripts import:

```python
# client_setup.py
from solana.rpc.api import Client
from solders.pubkey import Pubkey
import os

RPC_URL = os.environ.get("GETBLOCK_RPC") or "https://go.getblock.io/<ACCESS_TOKEN>"
client = Client(RPC_URL)
```

***

### Step 2: Fetch top holders (up to 20)

Get the largest token accounts for a mint. Call `getTokenLargestAccounts`, compute UI amounts from decimals, and return a list of holder records.

```python
# top_holders.py
from solana.rpc.api import Client
from solders.pubkey import Pubkey

client = Client(RPC_URL)

def get_top_holders(mint_str, commitment="finalized"):
    mint = Pubkey.from_string(mint_str)
    resp = client.get_token_largest_accounts(mint, commitment=commitment)
    if resp.get("error"):
        raise RuntimeError(resp["error"])
    value = resp["result"]["value"]  # list of dicts
    # each entry often contains: address, amount (string), decimals, uiAmount, uiAmountString
    # For robustness, compute UI amount using 'decimals' if present:
    holders = []
    for entry in value:
        addr = entry["address"]
        raw_amount_str = entry["amount"]
        decimals = entry.get("decimals")
        raw_amount = int(raw_amount_str)
        if decimals is not None:
            ui_amount = raw_amount / (10 ** int(decimals))
        else:
            # Fallback: if decimals not present, you should call get_token_supply() to get decimals
            ui_amount = raw_amount
        holders.append({"address": addr, "raw_amount": raw_amount, "decimals": decimals, "ui_amount": ui_amount})
    return holders

```

***

### Step 3: Get total supply & decimals&#x20;

Use `get_token_supply` to retrieve canonical decimals and total supply. This helps compute the holder share of supply.

```python
# supply.py
def get_mint_supply_and_decimals(mint_str):
    mint = Pubkey.from_string(mint_str)
    resp = client.get_token_supply(mint)
    if resp.get("error"):
        raise RuntimeError(resp["error"])
    value = resp["result"]["value"]
    total_raw = int(value["amount"])
    decimals = int(value["decimals"])
    total_ui = total_raw / (10 ** decimals)
    return {"total_raw": total_raw, "decimals": decimals, "total_ui": total_ui}

```

***

### Step 4: Visualize top holders

This example uses matplotlib and pandas to convert top-20 holders to a DataFrame, compute percent of supply and cumulative share, then plot.

```python
# visualize.py
import matplotlib.pyplot as plt
import pandas as pd

def plot_top_holders(holders, total_ui, top_n=20, title="Top token holders"):
    df = pd.DataFrame(holders)
    # ensure ui_amount exists
    if "ui_amount" not in df.columns and "ui_amount" in df.columns:
        df["ui_amount"] = df["ui_amount"]
    df = df.sort_values("ui_amount", ascending=False).head(top_n)
    df["pct_of_supply"] = df["ui_amount"] / total_ui * 100
    df["cumulative_pct"] = df["pct_of_supply"].cumsum()

    plt.figure(figsize=(12,6))

    # Bar: top holders
    plt.subplot(1,2,1)
    plt.bar(range(len(df)), df["ui_amount"])
    plt.xticks(range(len(df)), [a[:6] + "..." for a in df["address"]], rotation=45)
    plt.title(f"{title} — top {len(df)}")
    plt.ylabel("Balance (tokens)")

    # Line: cumulative %
    plt.subplot(1,2,2)
    plt.plot(df["cumulative_pct"], marker="o")
    plt.xticks(range(len(df)), [a[:6] + "..." for a in df["address"]], rotation=45)
    plt.title("Cumulative % of total supply")
    plt.ylabel("% of supply")
    plt.tight_layout()
    plt.show()

```

_**Example usage**_:

```python
if __name__ == "__main__":
    MINT = "4EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"  # USDC (mainnet)
    holders = get_top_holders(MINT)
    supply = get_mint_supply_and_decimals(MINT)
    plot_top_holders(holders, supply["total_ui"], top_n=10, title="USDC top holders")
```

{% hint style="info" %}
This chart shows **token accounts** ranked by balance, not necessarily **wallets**. A single wallet may control multiple token accounts. To aggregate per wallet, resolve each token account to its owner (via [getAccountInfo](https://docs.getblock.io/api-reference/solana-sol/getaccountinfo-solana) with `jsonParsed`), then sum balances per owner.
{% endhint %}

***

### Beyond the top-20: Fetching all token accounts for a mint

For a full distribution analysis, collect all token accounts and aggregate. Options:

* Call [`getProgramAccounts`](https://docs.getblock.io/api-reference/solana-sol/getprogramaccounts-solana) on the Token Program (`Tokenkeg...`) with a memcmp filter for the mint and decode account data using the SPL token account layout. Consult the SPL Token account layout for the correct offset of the mint field.
* Use [`getTokenAccountsByOwner`](https://docs.getblock.io/api-reference/solana-sol/gettokenaccountsbyowner-solana) for a given owner (not useful for all holders).
* Use an indexer add-on to your Solana node from GetBlock (recommended for >1000 accounts) or third-party APIs that maintain token holder lists.

**Example using `get_program_accounts`:**&#x20;

```python
from solana.rpc.types import MemcmpOpts

TOKEN_PROGRAM_ID = "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
MINT_OFFSET = 0  # offset inside token account binary where mint pubkey is stored (use correct offset)

def get_all_token_accounts_for_mint(mint_str):
    mint_bytes_base58 = Pubkey.from_string(mint_str).to_base58()
    # filter example (using memcmp against mint pubkey at the correct offset)
    filters = [MemcmpOpts(offset=0, bytes=mint_str),  # exact offset varies: consult token account layout
               ]
    resp = client.get_program_accounts(Pubkey.from_string(TOKEN_PROGRAM_ID), filters=filters)
    # parse accounts and extract balances (need to decode account.data using spl-token account layout)
    # This is more complex — consider using an indexer or spl-token lib to decode.

```

Decoding raw account data requires parsing the SPL Token account binary layout. For large-scale work, prefer an indexer or a specialized parser library.

***

### Performance, rate limits, and caching recommendations&#x20;

* Cache decimals and token metadata – they rarely change.
* For repeated analytics refreshes use incremental updates: fetch signatures for a mint and process only changed accounts, or subscribe to program logs for live dashboards.
* Running `getTokenLargestAccounts` or `getProgramAccounts` at scale can be heavy and may hit rate limits. Use a dedicated GetBlock Solana node or an indexer add-on to avoid throttling.

***

This tutorial shows practical methods to measure token-holder concentration on Solana. For quick exploration, the top-20 path is usually sufficient. For full distribution or production analytics, use program-account scans, an indexer, and a dedicated RPC node.
