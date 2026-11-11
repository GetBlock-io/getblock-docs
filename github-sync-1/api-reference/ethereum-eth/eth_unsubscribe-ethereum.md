---
description: >-
  Example code for the eth_unsubscribe JSON RPC method. Сomplete guide on how to
  use eth_unsubscribe JSON RPC in GetBlock Web3 documentation.
---

# eth\_unsubscribe - Ethereum

This method cancels a previously-created **WebSocket subscription**. The server immediately stops pushing notifications for that subscription ID. Required for well-behaved clients: subscriptions persist for the lifetime of the WebSocket connection unless explicitly unsubscribed.

## Parameters

| Parameter        | Type   | Required | Description                                             |
| ---------------- | ------ | -------- | ------------------------------------------------------- |
| `subscriptionId` | string | Yes      | Hex-encoded subscription ID returned by `eth_subscribe` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
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
    method: 'eth_unsubscribe',
    params: [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
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
        'method': 'eth_unsubscribe',
        'params': [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
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
            "method": "eth_unsubscribe",
            "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------- |
| `jsonrpc` | string  | JSON-RPC protocol version ("2.0")                                                       |
| `id`      | string  | Request identifier matching the request                                                 |
| `result`  | boolean | `true` if the subscription was found and cancelled, `false` if the ID wasn't recognized |

## Use Cases

* **Subscription Cleanup**: Cancel subscriptions when they're no longer needed to reduce server load
* **Dynamic Filter Refresh**: Unsubscribe and resubscribe with updated filter criteria
* **Connection Teardown**: Explicit cleanup before closing the WebSocket connection

## Error Handling

| Error Code | Message        | Description                            |
| ---------- | -------------- | -------------------------------------- |
| -32602     | Invalid params | Subscription ID is malformed           |
| -32603     | Internal error | Node failed to cancel the subscription |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// High-level: remove listeners
const handler = (blockNumber) => console.log('New block:', blockNumber);
provider.on('block', handler);
// Later:
provider.off('block', handler);

// Raw eth_unsubscribe:
const cancelled = await provider.send('eth_unsubscribe', ['0x9cef478923ff08bf67fde6c64013158d']);
console.log('Cancelled:', cancelled);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: webSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

// watchBlocks / watchEvent return an unwatch function
const unwatch = client.watchBlocks({
    onBlock: (block) => console.log('New block:', block.number),
});

// Later, cancel the subscription:
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
