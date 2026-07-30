---
description: >-
  Example code for the eth_unsubscribe json-rpc method. Сomplete guide on how to
  use eth_unsubscribe json-rpc in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Polygon

The **eth\_unsubscribe** method cancels a subscription created with eth\_subscribe. The subscription will no longer send notifications after this call.

{% hint style="info" %}
This method requires a WebSocket connection.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                |
| -------------- | ------ | -------- | ------------------------------------------ |
| subscriptionId | string | Yes      | Subscription ID returned by eth\_subscribe |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "eth_unsubscribe", "params": ["0x9cef478923ff08bf67fde6c64013158d"], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
    "id": "getblock.io"
}));
});

ws.on('message', (data) => {
    console.log(JSON.parse(data.toString()));
});
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import asyncio
import json
import websockets

async def main():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0x9cef478923ff08bf67fde6c64013158d"
    ],
    "id": "getblock.io"
}))
        async for message in ws:
            print(json.loads(message))

asyncio.run(main())
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use tokio_tungstenite::connect_async;
use tokio_tungstenite::tungstenite::Message;
use serde_json::json;
use futures_util::{SinkExt, StreamExt};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let (mut ws_stream, _) = connect_async("wss://go.getblock.io/<ACCESS-TOKEN>/").await?;

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": [
                "0x9cef478923ff08bf67fde6c64013158d"
        ],
        "id": "getblock.io"
});
    ws_stream.send(Message::Text(payload.to_string())).await?;

    while let Some(msg) = ws_stream.next().await {
        println!("{:?}", msg?);
    }
    Ok(())
}
```
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

| Field   | Type   | Description                |
| ------- | ------ | -------------------------- |
| jsonrpc | string | JSON-RPC version (2.0)     |
| id      | string | Request identifier         |
| result  | varies | Boolean indicating success |

## Use Case

The eth\_unsubscribe method is useful for:

* **Subscription cleanup**
* **Resource management**

## Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32600      | Invalid Request | Malformed request body          |
| -32602      | Invalid params  | Invalid method parameters       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const result = await provider.send('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"]);
console.log('Result:', result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { polygon } from 'viem/chains';

const client = createPublicClient({
    chain: polygon,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_unsubscribe',
    params: ["0x9cef478923ff08bf67fde6c64013158d"]
});
console.log('Result:', result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
