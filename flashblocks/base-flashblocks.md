---
description: >-
  Flashblocks-specific RPC methods, WebSocket subscriptions, and the
  infrastructure stream schema for Base pre-confirmations.
---

# Base Flashblocks

Flashblocks deliver \~200ms transaction preconfirmations on Base by streaming partial blocks before the next full block is sealed. Each 2-second Base block is divided into ten Flashblocks streamed at 200ms intervals, exposing preconfirmed state through the standard JSON-RPC `pending` block tag.

### Endpoints

| Transport | URL                                      |
| --------- | ---------------------------------------- |
| HTTP      | `https://go.getblock.io/<ACCESS-TOKEN>/` |
| WebSocket | `wss://go.getblock.io/<ACCESS-TOKEN>/`   |

{% hint style="info" %}
The `pending` tag works over HTTP. Flashblocks subscriptions require the WebSocket endpoint.
{% endhint %}

### Flashblocks-Aware Methods

| Method                          | Usage                                                                  |
| ------------------------------- | ---------------------------------------------------------------------- |
| `eth_getBlockByNumber`          | Pass `"pending"` to retrieve the current Flashblock                    |
| `eth_getBalance`                | Pass `"pending"` for the balance at the latest Flashblock state        |
| `eth_getTransactionCount`       | Pass `"pending"` for a nonce that accounts for Flashblock transactions |
| `eth_call`                      | Pass `"pending"` to execute against preconfirmed state                 |
| `eth_estimateGas`               | Pass `"pending"` to estimate gas against preconfirmed state            |
| `eth_getCode`                   | Pass `"pending"` for contract code at preconfirmed state               |
| `eth_getStorageAt`              | Pass `"pending"` for storage at preconfirmed state                     |
| `eth_getBlockTransactionByHash` | Returns preconfirmed block transaction data (no tag required)          |
| `eth_simulateV1`                | Simulates against the latest preconfirmed state                        |
| `base_transactionStatus`        | Reports whether a transaction hash is present in the mempool           |
| `eth_subscribe`                 | Opens a Flashblocks WebSocket subscription                             |
| `eth_unsubscribe`               | Cancels an active Flashblocks subscription                             |
| `eth_getLogs`                   | Get array of all logs matching a given filter object.                  |

### Example

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

{% tab title="Request(Python)" %}
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

#### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3bdf5275488a29a8a57c"
}
```

### Use Cases

* **Instant UI Feedback**: Update wallet and dApp interfaces the moment a transaction is preconfirmed, without waiting for a sealed block.
* **Responsive Swaps**: Surface preconfirmed swap results on decentralized exchanges with sub-second latency.
* **Onchain Gaming**: Reflect game state changes as soon as actions are included in a Flashblock.
* **Nonce Management**: Use `eth_getTransactionCount` with `pending` to avoid nonce collisions when submitting transactions in rapid succession.
* **Preconfirmed Receipts**: Poll `eth_getTransactionReceipt` on a 200ms interval to confirm inclusion at Flashblock cadence.
