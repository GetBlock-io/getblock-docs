---
description: >-
  Flashblocks-specific RPC methods, WebSocket subscriptions, and the
  infrastructure stream schema for Base pre-confirmations.
---

# Base Flashblocks

Flashblocks deliver \~200ms transaction preconfirmations on Base by streaming partial blocks before the next full block is sealed. Each 2-second Base block is divided into ten Flashblocks streamed at 200ms intervals, exposing preconfirmed state through the standard JSON-RPC `pending` block tag.

Base runs on the OP Stack (chain ID 8453). Flashblocks have been live on Base Mainnet since July 2025 and are also available on Base Sepolia (chain ID 84532). The feature is served over the standard Base RPC interface, so no separate endpoint or add-on is required to read preconfirmed state.

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
| `base_transactionStatus`    | Reports whether a transaction hash is present in the mempool           |
| `eth_subscribe`             | Opens a Flashblocks WebSocket subscription                             |
| `eth_unsubscribe`           | Cancels an active Flashblocks subscription                             |

## Reading Preconfirmed State

The example below reads the `pending` balance of the Base WETH contract `0x4200000000000000000000000000000000000006`. Substitute any supported method and parameters; the `"pending"` tag is what selects Flashblock state.

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
    "result": "0x3bdf5275488a29a8a57c"
}
```

The `result` is the hex-encoded balance in wei at the latest Flashblock. The same request with the `"latest"` tag returns the value at the last sealed block.

## Streaming Flashblocks over WebSocket

Open a WebSocket connection and subscribe to `newFlashblocks` to receive each Flashblock as it is built, roughly every 200ms. Subscription notifications arrive as `eth_subscription` messages. Use `newFlashblockTransactions` to stream individual preconfirmed transactions, or `pendingLogs` to stream event logs from preconfirmed transactions.

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
        // Fires every ~200ms with the latest Flashblock state
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
    "result": "0x3b8cd9e5f4a7b2c1d0e3f4a5b6c7d8e9"
}
```

Each notification payload is a Flashblock object containing `payload_id`, `index`, `diff`, and, on `index` 0, the `base` block context. If per-event handling is heavy, throttle or debounce the consumer to avoid blocking, since events arrive at 5 Hz.

## Confirming Submission with base\_transactionStatus

`base_transactionStatus` is a Base-specific method that reports whether a transaction hash is present in the node mempool. It is useful for confirming that a submitted transaction has been received before a preconfirmation lands.

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "base_transactionStatus",
    "params": ["0x2c4c4979e226139eb70762c9ffee2920e7e82571e512ea5712c43d55cdb26eb4"],
    "id": "getblock.io"
}'
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
        'method': 'base_transactionStatus',
        'params': ['0x2c4c4979e226139eb70762c9ffee2920e7e82571e512ea5712c43d55cdb26eb4'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

A `"Known"` status means the transaction is in the mempool; `"Unknown"` None

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "status": "Known"
    }
}
```

## Use Cases

* **Instant UI Feedback**: Update wallet and dApp interfaces the moment a transaction is preconfirmed, without waiting for a sealed block.
* **Responsive Swaps**: Surface preconfirmed swap results on decentralized exchanges with sub-second latency.
* **Onchain Gaming**: Reflect game state changes as soon as actions are included in a Flashblock.
* **Nonce Management**: Use `eth_getTransactionCount` with `pending` to avoid nonce collisions when submitting transactions in rapid succession.
* **Preconfirmed Receipts**: Poll `eth_getTransactionReceipt` on a 200ms interval to confirm inclusion at Flashblock cadence.

## Web3 Integration

Both Ethers.js and Viem accept the `pending` block tag on read methods, so Flashblock state is available through the standard provider interface.

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
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
{% code title="viem-example.js" overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
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
