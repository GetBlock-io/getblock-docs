---
description: Step-by-step guide to getting a fast, reliable Solana RPC endpoint.
---

# How to Get a Solana RPC Endpoint

Solana is the go-to chain for high-frequency trading, DeFi, and real-time applications — which means your RPC infrastructure needs to keep up.&#x20;

A Solana RPC endpoint is a URL that connects your application to a Solana validator node. Through this endpoint, your app can:

* Query account balances and token holdings (`getBalance`, `getTokenAccountsByOwner`)
* Fetch transaction data (`getTransaction`, `getSignaturesForAddress`)
* Submit transactions (`sendTransaction`)
* Monitor real-time events via WebSocket (`accountSubscribe`, `logsSubscribe`)
* Access slot and block information (`getSlot`, `getBlock`)

Solana's JSON-RPC API follows a different specification than Ethereum — it's not EVM-based, so you'll use Solana-specific libraries like `@solana/web3.js` or `solana-py`.

**Endpoint format:**

```bash
https://go.getblock.io/<YOUR-ACCESS-TOKEN>/
```

### Step-by-Step: Get Your Ethereum RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create an Ethereum Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

3. Select:
   * **Protocol:** Solana (SOL)
   * **Network:** Mainnet-beta (or Devnet/Testnet for development)
   * **API Interface:** JSON-RPC or WebSocket or MEV protected (JSON-RPC)
   * **Region:** Frankfurt (EU), New York (US), or Singapore (APAC)

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

4. Click **"Create":** Your endpoint URL will be generated immediately.
{% endstep %}

{% step %}
#### Copy Your Endpoint URL

Your endpoint URL looks like this:

```
https://go.getblock.io/a1b2c3d4e5f6789012345678abcdef01/
```

{% hint style="warning" %}
The long string after `go.getblock.io/` is your **access token** — keep it private.
{% endhint %}
{% endstep %}

{% step %}
#### Test the Connection

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<YOUR-ACCESS-TOKEN>/ \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSlot",
    "params": []
  }'
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "result": 298765432,
  "id": 1
}
```
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Examples

{% tabs %}
{% tab title="JavaScript (@solana/web3.js)" %}
```javascript
import { Connection, PublicKey, LAMPORTS_PER_SOL } from "@solana/web3.js";

const connection = new Connection(
  "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/",
  "confirmed"
);

// Get current slot
const slot = await connection.getSlot();
console.log("Current slot:", slot);

// Get SOL balance
const pubkey = new PublicKey("7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU");
const balance = await connection.getBalance(pubkey);
console.log(`Balance: ${balance / LAMPORTS_PER_SOL} SOL`);

// Get recent blockhash (needed for transactions)
const { blockhash } = await connection.getLatestBlockhash();
console.log("Blockhash:", blockhash);
```
{% endtab %}

{% tab title="Python (solana-py)" %}
```python
from solana.rpc.api import Client
from solders.pubkey import Pubkey

client = Client("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/")

# Get current slot
slot = client.get_slot()
print(f"Current slot: {slot.value}")

# Get SOL balance
pubkey = Pubkey.from_string("7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU")
balance = client.get_balance(pubkey)
print(f"Balance: {balance.value / 1e9} SOL")

# Get recent transactions for an address
sigs = client.get_signatures_for_address(pubkey, limit=5)
for sig in sigs.value:
    print(f"  TX: {sig.signature}")
```
{% endtab %}

{% tab title="Python (requests — no SDK)" %}
```python
import requests

url = "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"
headers = {"Content-Type": "application/json"}

# Get token accounts for a wallet
payload = {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTokenAccountsByOwner",
    "params": [
        "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
        {"programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"},
        {"encoding": "jsonParsed"}
    ]
}

response = requests.post(url, headers=headers, json=payload)
accounts = response.json()["result"]["value"]
for acc in accounts:
    info = acc["account"]["data"]["parsed"]["info"]
    print(f"Token: {info['mint']}, Amount: {info['tokenAmount']['uiAmountString']}")
```
{% endtab %}

{% tab title="Rust (solana-client)" %}
```rust
use solana_client::rpc_client::RpcClient;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

fn main() {
    let client = RpcClient::new("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/");
    
    // Get current slot
    let slot = client.get_slot().unwrap();
    println!("Current slot: {}", slot);
    
    // Get balance
    let pubkey = Pubkey::from_str("7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU").unwrap();
    let balance = client.get_balance(&pubkey).unwrap();
    println!("Balance: {} SOL", balance as f64 / 1e9);
}
```
{% endtab %}
{% endtabs %}

### WebSocket for Real-Time Solana Data

Solana's WebSocket API is essential for real-time monitoring:

{% tabs %}
{% tab title="Base URL" %}
```bash
wss://go.getblock.io/<YOUR-ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Subscribe to account changes" %}
```javascript
import WebSocket from "ws";

const ws = new WebSocket("wss://go.getblock.io/<YOUR-ACCESS-TOKEN>/");

ws.on("open", () => {
  // Subscribe to SOL balance changes for an address
  ws.send(JSON.stringify({
    jsonrpc: "2.0",
    id: 1,
    method: "accountSubscribe",
    params: [
      "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
      { encoding: "jsonParsed", commitment: "confirmed" }
    ]
  }));
});

ws.on("message", (data) => {
  const msg = JSON.parse(data);
  if (msg.method === "accountNotification") {
    console.log("Account updated:", msg.params.result.value);
  }
});
```
{% endtab %}

{% tab title="SubscSubcribe to program logs (e.g., monitor DEX trades)" %}
```javascript
// Monitor Raydium AMM trades
ws.send(JSON.stringify({
  jsonrpc: "2.0",
  id: 2,
  method: "logsSubscribe",
  params: [
    { mentions: ["675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"] },  // Raydium v4
    { commitment: "confirmed" }
  ]
}));
```
{% endtab %}
{% endtabs %}

### Why You Need a Dedicated RPC Provider for Solana

Solana is especially demanding on RPC infrastructure:

| Challenge                       | Public RPC              | GetBlock               |
| ------------------------------- | ----------------------- | ---------------------- |
| Rate limits                     | Aggressive (2-5 RPS)    | Up to 500 RPS          |
| `getSignaturesForAddress` depth | Often limited to recent | Full history available |
| WebSocket stability             | Frequent disconnects    | Persistent connections |
| `getProgramAccounts` support    | Usually blocked         | Available              |
| MEV protection                  | None                    | Available (Dedicated)  |
| gRPC streaming (Yellowstone)    | Not available           | Available as add-on    |
| Archive/historical data         | Limited                 | Full history           |
| Transaction landing rate        | Low priority            | High via LandFirst     |

### Advanced Solana Infrastructure on GetBlock

#### 1. Yellowstone gRPC (Geyser Plugin)

For applications that need the fastest possible data e.g., trading bots, MEV searchers, indexers. GetBlock offers managed Yellowstone gRPC endpoints as an add-on to Dedicated Solana Nodes.

**Why Yellowstone gRPC over standard RPC:**

* **Near-zero latency:** streams data directly from validators
* **Millions of events per minute:** handles Solana's full throughput
* **Rich filtering:** subscribe only to accounts/programs you care about
* **Protobuf encoding:** parsed, typed data instead of base64

[_Learn more about Yellowstone gRPC_](https://docs.getblock.io/add-ons/yellowstone-grpc-api)

#### 2. StreamFirst: Ultra-Low Latency Data

GetBlock's proprietary StreamFirst infrastructure delivers on-chain data faster than standard Yellowstone by combining:

* **Accelerated Yellowstone gRPC** with optimized serialization
* **Shred-stream delivery:** receives block fragments directly via UDP before blocks are fully confirmed

The Frankfurt data center provides 6ms latency within Europe and is positioned near the highest density of Solana validators.

[_Learn more about StreamFirst_](https://docs.getblock.io/solana-advanced-data-tools/streamfirst)

#### 3. LandFirst: Smart Transaction Routing

For sending transactions with high landing probability:

* **SWQoS connections** to high-stakes validators
* **Jito Block Engine integration** for bundle support
* **Intelligent routing** based on leader schedule and network conditions

[_Learn more about LandFirst_](https://docs.getblock.io/solana-advanced-data-tools/landfirst)

#### 4. TradeFirst — Complete HFT Infrastructure

Combines StreamFirst (data) + LandFirst (execution) + Jito integration for high-frequency trading:

* See opportunities \~17ms faster than standard RPC
* Execute trades via optimal routing paths
* Bundle support for atomic multi-transaction operations

[_Learn more about TradeFirst_](https://docs.getblock.io/solana-advanced-data-tools/tradefirst)

### What's Next?

* [Full Solana API Reference ](https://docs.getblock.io/api-reference/solana-sol)
* [Yellowstone gRPC Quickstart ](https://docs.getblock.io/add-ons/yellowstone-grpc-api/quickstart-guide)
* [How to Track Pump.fun Token Mints](https://docs.getblock.io/guides/how-to-track-pump.fun-token-mints-with-getblocks-yellowstone-grpc)
* [Sniping New Solana Tokens (Python Tutorial)](https://docs.getblock.io/guides/blockchain-api-guides/solana-guides/sniping-new-solana-tokens-python-bot-tutorial)
* [Solana Indexed Archive](https://docs.getblock.io/solana-advanced-data-tools/solana-indexed-archive)
* [Configure a Dedicated Solana Node](https://getblock.io/dedicated-nodes/)

_Building something ambitious on Solana?_ [_Talk to our team_](mailto:support@getblock.io) _about custom infrastructure — we specialize in high-performance Solana deployments._
