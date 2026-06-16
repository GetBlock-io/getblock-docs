---
hidden: true
---

# optimism flashblocks

Flashblocks deliver near real-time transaction preconfirmations on Optimism by streaming partial blocks before the next full block is sealed. Preconfirmed state is exposed through the standard JSON-RPC `pending` block tag, reducing perceived confirmation time from the standard \~2-second block interval to roughly 250 milliseconds.

Optimism runs on the OP Stack (chain ID 10), the reference implementation that Flashblocks' Rollup-Boost sidecar was built for. Flashblocks are available on Optimism Mainnet and Optimism Sepolia (chain ID 11155420), served over the standard Optimism RPC interface so existing tooling reads preconfirmed state without modification.

## Endpoints

| Transport | URL                                      |
| --------- | ---------------------------------------- |
| HTTP      | `https://go.getblock.io/<ACCESS-TOKEN>/` |
| WebSocket | `wss://go.getblock.io/<ACCESS-TOKEN>/`   |

The `pending` tag works over HTTP. Flashblocks subscriptions require the WebSocket endpoint.

## Flashblocks-Aware Methods

| Method                      | Usage                                                                  |
| --------------------------- | ---------------------------------------------------------------------- |
| `eth_getBlockByNumber`      | Pass `"pending"` to retrieve the current Flashblock                    |
| `eth_getBalance`            | Pass `"pending"` for the balance at the latest Flashblock state        |
| `eth_getTransactionCount`   | Pass `"pending"` for a nonce that accounts for Flashblock transactions |
| `eth_call`                  | Pass `"pending"` to execute against preconfirmed state                 |
| `eth_estimateGas`           | Pass `"pending"` to estimate gas against preconfirmed state            |
| `eth_getCode`               | Pass `"pending"` for contract code at preconfirmed state               |
| `eth_getStorageAt`          | Pass `"pending"` for storage at preconfirmed state                     |
| `eth_getTransactionReceipt` | Returns receipts for preconfirmed transactions (no tag required)       |
| `eth_getTransactionByHash`  | Returns preconfirmed transaction data (no tag required)                |
| `eth_simulateV1`            | Simulates against the latest preconfirmed state                        |
| `eth_subscribe`             | Opens a Flashblocks WebSocket subscription                             |
| `eth_unsubscribe`           | Cancels an active Flashblocks subscription                             |

## Reading Preconfirmed State

The example below reads the `pending` balance of the Optimism WETH contract `0x4200000000000000000000000000000000000006`. Substitute any supported method and parameters; the `"pending"` tag is what selects Flashblock state.

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x4200000000000000000000000000000000000006", "pending"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBalance',
    params: ['0x4200000000000000000000000000000000000006', 'pending'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBalance',
        'params': ['0x4200000000000000000000000000000000000006', 'pending'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBalance",
            "params": ["0x4200000000000000000000000000000000000006", "pending"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0de0b6b3a7640000"
}
```

The `result` is the hex-encoded balance in wei at the latest Flashblock. The same request with the `"latest"` tag returns the value at the last sealed block.

## Streaming Flashblocks over WebSocket

Open a WebSocket connection and subscribe to `newFlashblocks` to receive each Flashblock as it is built. Subscription notifications arrive as `eth_subscription` messages. Use `newFlashblockTransactions` to stream individual preconfirmed transactions, or `pendingLogs` to stream event logs from preconfirmed transactions.

{% code title="subscribe.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
        jsonrpc: '2.0',
        method: 'eth_subscribe',
        params: ['newFlashblocks'],
        id: 'getblock.io'
    }));
});

ws.on('message', (data) => {
    const msg = JSON.parse(data.toString());
    if (msg.method === 'eth_subscription') {
        // Fires as each Flashblock is produced
        console.log(msg.params.result);
    }
});
```
{% endcode %}

The subscription confirmation returns a hex-encoded subscription ID:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9f2a7c1e8b4d6a3f5c0e2d1b8a7f6e5d"
}
```

Each notification payload is a Flashblock object carrying the new transactions and state changes since the previous Flashblock. Applications should treat the stream as an optimization over the RPC interface and avoid a hard dependency on it, since the RPC `pending` tag provides automatic fallback to standard blocks if Flashblocks become unavailable.

## Use Cases

* **Trading Platforms**: Surface preconfirmed order and swap results with sub-second latency.
* **Prediction Markets**: Reflect market state changes as soon as transactions are preconfirmed.
* **Wallets**: Show instant transaction feedback and real-time balance updates before a block is sealed.
* **Nonce Management**: Use `eth_getTransactionCount` with `pending` to avoid nonce collisions during rapid submission.
* **Preconfirmed Receipts**: Poll `eth_getTransactionReceipt` to confirm inclusion at Flashblock cadence.

## Web3 Integration

Both Ethers.js and Viem accept the `pending` block tag on read methods, so Flashblock state is available through the standard provider interface.

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Read preconfirmed balance from the latest Flashblock
const balance = await provider.getBalance(
    '0x4200000000000000000000000000000000000006',
    'pending'
);

console.log(ethers.formatEther(balance));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Read preconfirmed balance from the latest Flashblock
const balance = await client.getBalance({
    address: '0x4200000000000000000000000000000000000006',
    blockTag: 'pending'
});

console.log(balance);
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Flashblocks and Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Optimism documentation portal at docs.optimism.io.
{% endhint %}
