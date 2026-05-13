---
description: >-
  Example code for the eth_unsubscribe json-rpc method. Сomplete guide on how to
  use eth_unsubscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Optimism

This method cancels a subscription that was created with `eth_subscribe`. Once cancelled, no more notifications will be sent for that subscription. This should be called when real-time updates are no longer needed.

{% hint style="warning" %}
The **eth\_unsubscribe** method cancels an existing WebSocket subscription on Optimism.
{% endhint %}

### Parameters

| Parameter      | Type   | Description                    | Required |
| -------------- | ------ | ------------------------------ | -------- |
| subscriptionId | String | The subscription ID to cancel. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL (wscat)" %}
```bash
# This method requires WebSocket connection
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

> {"jsonrpc":"2.0","method":"eth_unsubscribe","params":["0x1"],"id":"getblock.io"}
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
{% code title="JavaScript (ws)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
        jsonrpc: '2.0',
        method: 'eth_unsubscribe',
        params: ['0x1'],
        id: 'getblock.io'
    }));
});

ws.on('message', (data) => {
    console.log(JSON.parse(data));
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python (websockets)" %}
```python
import asyncio
import websockets
import json

async def unsubscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as ws:
        payload = {
            "jsonrpc": "2.0",
            "method": "eth_unsubscribe",
            "params": ["0x1"],
            "id": "getblock.io"
        }
        await ws.send(json.dumps(payload))
        response = await ws.recv()
        print(json.loads(response))

asyncio.run(unsubscribe())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (tokio-tungstenite)" %}
```rust
use tokio_tungstenite::connect_async;
use futures_util::{SinkExt, StreamExt};
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "wss://go.getblock.io/<ACCESS-TOKEN>/";
    let (mut ws, _) = connect_async(url).await?;
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": ["0x1"],
        "id": "getblock.io"
    });
    
    ws.send(payload.to_string().into()).await?;
    
    if let Some(msg) = ws.next().await {
        println!("{:?}", msg?);
    }
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: Boolean indicating whether the subscription was successfully cancelled.

### Use Case

The eth\_unsubscribe method is commonly used for:

* **Subscription management**
* **Resource cleanup**
* **Connection lifecycle management**

## Error Handling

| Error Code | Description             |
| ---------- | ----------------------- |
| -32602     | Invalid subscription ID |
| -32603     | Internal error          |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Subscribe to new blocks
provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});

// Later: unsubscribe
provider.removeAllListeners('block');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
    onBlock: block => console.log(block.number)
});

// Later: unsubscribe
unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
