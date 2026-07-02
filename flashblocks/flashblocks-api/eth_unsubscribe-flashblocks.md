---
description: >-
  Example code for the eth_unsubscribe Flashblocks method. Complete guide on how
  to use eth_unsubscribe Flashblocks in GetBlock Web3 documentation.
---

# eth\_unsubscribe - Flashblocks

This cancels a subscription created with `eth_subscribe`. The subscription ID is no longer valid after this call.

| Parameter      | Type   | Required | Description                                      |
| -------------- | ------ | -------- | ------------------------------------------------ |
| subscriptionId | string | Yes      | The subscription ID returned by `eth_subscribe`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "eth_unsubscribe", "params": ["0x9cef478923ff08bf67fde6c64013158d"], "id": "getblock.io"}
```
{% endcode %}
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

### Response

{% code overflow="wrap" %}
```bash
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": true
}
```
{% endcode %}

## Response Parameters

| Field  | Type    | Description                                                                                          |
| ------ | ------- | ---------------------------------------------------------------------------------------------------- |
| result | boolean | `true` if the subscription was successfully cancelled, `false` if the subscription ID was not found. |

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |
| -32600      | Invalid Request   | Subscription called over HTTP; use WebSocket   |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Flashblocks subscription types aren't first-class in ethers — use the raw send interface:
const subscriptionId = await provider.send('eth_usubscribe', ["0xf48bfa9af6a7f77897a0f0e59c1061bc"]);
console.log('Subscribed:', subscriptionId);
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';

const client = createPublicClient({
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

// Flashblocks subscription types use the raw request interface:
const subscriptionId = await client.request({
   "method": "eth_unsubscribe", 
    "params": ["0xf48bfa9af6a7f77897a0f0e59c1061bc"]
});
console.log('Subscribed:', subscriptionId);
```
{% endtab %}
{% endtabs %}
