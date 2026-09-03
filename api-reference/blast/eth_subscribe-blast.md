# eth\_subscribe - Blast

This method opens a push subscription over a WebSocket connection and returns a subscription ID. Supported event types are newHeads, logs, newPendingTransactions, and syncing. It requires a wss:// endpoint rather than HTTP.

{% hint style="info" %}
This method works only over a WebSocket connection. Use the `wss://` form of the endpoint (`wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`) rather than HTTPS.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                            |
| ---------------- | ------ | -------- | ------------------------------------------------------ |
| subscriptionType | string | Yes      | One of newHeads, logs, newPendingTransactions, syncing |
| options          | object | No       | Filter options (used with the logs subscription)       |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_subscribe",
    "params": ["logs", { "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
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
    method: 'eth_subscribe',
    params: ['logs', { address: '0x4200000000000000000000000000000000000006', topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
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
        'method': 'eth_subscribe',
        'params': ['logs', { 'address': '0x4200000000000000000000000000000000000006', 'topics': ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }],
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
            "method": "eth_subscribe",
            "params": ["logs", { "address": "0x4200000000000000000000000000000000000006", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }],
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

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | string | Subscription ID used to correlate pushed notifications |

## Use Cases

* **Live Heads**: Receive newHeads pushes to follow the \~1 second block cadence
* **Event Streams**: Stream matching logs without polling
* **Mempool Feeds**: Receive newPendingTransactions as they arrive
* **Sync Monitoring**: Watch syncing state transitions in real time

## Error Handling

| Error Code | Message          | Description                                            |
| ---------- | ---------------- | ------------------------------------------------------ |
| -32602     | Invalid params   | Unknown subscription type or malformed options         |
| -32601     | Method not found | The endpoint was called over HTTP instead of WebSocket |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const provider = new ethers.WebSocketProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
provider.on('block', (n) => console.log(n));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const client = createPublicClient({ transport: webSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });
client.watchBlocks({ onBlock: (b) => console.log(b) });
```
{% endcode %}
{% endtab %}
{% endtabs %}
