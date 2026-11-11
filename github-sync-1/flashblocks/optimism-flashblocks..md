---
description: >-
  Flashblocks-specific RPC methods, WebSocket subscriptions, and the
  infrastructure stream schema for Optimism pre-confirmations.
---

# Optimism Flashblocks

Optimism runs on the OP Stack (chain ID 10), the reference implementation for which Flashblocks' Rollup-Boost sidecar was built. Flashblocks are available on Optimism Mainnet and Optimism Sepolia (chain ID 11155420), served over the standard Optimism RPC interface so existing tooling reads preconfirmed state without modification.

### Endpoints

| Transport | URL                                      |
| --------- | ---------------------------------------- |
| HTTP      | `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` |
| WebSocket | `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`   |

{% hint style="info" %}
The `pending` tag works over HTTP. Flashblocks subscriptions require the WebSocket endpoint.
{% endhint %}

### Flashblocks-Aware Methods

<table data-search="false"><thead><tr><th>Method</th><th>Usage</th></tr></thead><tbody><tr><td><code>eth_getTransactionCount</code></td><td>Pass <code>"pending"</code> for a nonce that accounts for Flashblock transactions</td></tr><tr><td><code>eth_getStorageAt</code></td><td>Pass <code>"pending"</code> for storage at preconfirmed state</td></tr><tr><td><code>eth_getBalance</code></td><td>Pass <code>"pending"</code> for the balance at the latest Flashblock state</td></tr><tr><td><code>eth_getBlockByNumber</code></td><td>Pass <code>"pending"</code> to retrieve the current Flashblock</td></tr><tr><td><code>eth_getBlockReceipt</code></td><td>Pass <code>"pending"</code> to get receipts for all pre-confirmed transactions in the current Flashblock.</td></tr><tr><td><code>eth_getBlockTransactionCountByNumber</code></td><td>Pass <code>"pending"</code> to get the count of pre-confirmed transactions in the current Flashblock.</td></tr><tr><td><code>eth_estimateGas</code></td><td>Pass <code>"pending"</code> to estimate gas against preconfirmed state</td></tr><tr><td><code>eth_call</code></td><td>Pass <code>"pending"</code> to execute against preconfirmed state</td></tr><tr><td><code>eth_simulateV1</code></td><td>Simulates against the latest preconfirmed state</td></tr><tr><td><code>eth_getLogs</code></td><td>Pass  <code>"fromBlock": "pending"</code> and <code>"toBlock": "pending"</code> to query logs from pre-confirmed transactions, updated every ~200ms.</td></tr><tr><td><code>eth_getCode</code></td><td>Pass <code>"pending"</code> for contract code at preconfirmed state</td></tr><tr><td><code>eth_subscribe</code></td><td>Opens a Flashblocks WebSocket subscription</td></tr><tr><td><code>eth_unsubscribe</code></td><td>Cancels an active Flashblocks subscription</td></tr></tbody></table>

### Example

The example below reads the `pending` balance of the Optimism WETH contract `0x4200000000000000000000000000000000000006`. Substitute any supported method and parameters; the `"pending"` tag is what selects Flashblock state.

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
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

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
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
    "result": "0x0de0b6b3a7640000"
}
```

The `result` is the hex-encoded balance in wei at the latest Flashblock. The same request with the `"latest"` tag returns the value at the last sealed block.

## Use Cases

* **Trading Platforms**: Surface preconfirmed order and swap results with sub-second latency.
* **Prediction Markets**: Reflect changes in market state as soon as transactions are preconfirmed.
* **Wallets**: Show instant transaction feedback and real-time balance updates before a block is sealed.
* **Nonce Management**: Use `eth_getTransactionCount` with `pending` to avoid nonce collisions during rapid submission.
* **Preconfirmed Receipts**: Poll `eth_getTransactionReceipt` to confirm inclusion at Flashblock cadence.
