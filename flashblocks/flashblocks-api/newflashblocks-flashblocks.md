---
description: >-
  Example code for the newFlashblocks Flashblocks method. Complete guide on how
  to use newFlashblocks Flashblocks in GetBlock Web3 documentation.
---

# newFlashblocks - Flashblocks

This subscribes to the full Flashblock payload stream as each preconfirmed sub-block is built. Each notification delivers a Flashblock Object containing `payload_id`, `index` (0-9 on Base, 0-7 on Optimism), `diff` (the delta since the previous Flashblock), and — on the first Flashblock of a new block (`index: 0`) — a `base` field with the block's initial state. This is the lowest-latency Flashblocks subscription and the canonical way to build sub-block indexers.

{% hint style="warning" %}
**WebSocket-only method.** This method requires the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/`. It will not work via HTTP POST. Preconfirmed events arrive at the Flashblock cadence — approximately every 200ms on Base, 250ms on Optimism.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                |
| ---------------- | ------ | -------- | -------------------------- |
| subscriptionType | string | Yes      | Must be `"newFlashblocks"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "newFlashblocks", "params": ["newFlashblocks"], "id": "getblock.io"}
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
    "method": "newFlashblocks",
    "params": [
        "newFlashblocks"
    ],
    "id": "getblock.io"
}));
});

ws.on('message', (data) => {
    const msg = JSON.parse(data.toString());
    if (msg.method === 'eth_subscription') {
        // Preconfirmed data update
        console.log(msg.params.result);
    } else {
        // Subscription ID response
        console.log('Subscribed:', msg.result);
    }
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
    "method": "newFlashblocks",
    "params": [
        "newFlashblocks"
    ],
    "id": "getblock.io"
}))
        async for message in ws:
            msg = json.loads(message)
            if msg.get('method') == 'eth_subscription':
                # Preconfirmed data update
                print(msg['params']['result'])
            else:
                # Subscription ID response
                print('Subscribed:', msg.get('result'))

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
        "method": "newFlashblocks",
        "params": [
                "newFlashblocks"
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

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3b8cd9e5f4a7b2c1d0e3f4a5b6c7d8e9"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                                                                                                  |
| ------ | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| result | string | Hex-encoded subscription ID. Each subsequent `eth_subscription` notification carries a Flashblock Object payload with `payload_id`, `index`, `diff`, and (on index 0) `base` |

## Use Cases

* Sub-block indexers — the primary streaming source for Flashblocks-native data
* Real-time analytics dashboards showing block construction in progress
* Trading infrastructure that needs the earliest possible view of sequencer-ordered transactions
* MEV monitoring — observing preconfirmed transactions as they land

## Error Handling

| Status Code | Error Message     | Cause                                                         |
| ----------- | ----------------- | ------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                           |
| -32602      | Invalid params    | Request parameters are missing or malformed                   |
| -32603      | Internal error    | Server-side error while processing the request                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                             |
| -32600      | Invalid Request   | `newFlashblocks` subscription called over HTTP; use WebSocket |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Flashblocks subscription types aren't first-class in ethers — use the raw send interface:
const subscriptionId = await provider.send('eth_subscribe', ["newFlashblocks"]);
console.log('Subscribed:', subscriptionId);

// Listen for incoming events on the WebSocket transport
provider.websocket.addEventListener('message', (event) => {
    const msg = JSON.parse(event.data);
    if (msg.method === 'eth_subscription' && msg.params?.subscription === subscriptionId) {
        console.log('Flashblocks update:', msg.params.result);
    }
});
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';

const client = createPublicClient({
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

// Flashblocks subscription types use the raw request interface:
const subscriptionId = await client.request({
    method: 'eth_subscribe',
    params: ["newFlashblocks"]
});
console.log('Subscribed:', subscriptionId);
```
{% endtab %}
{% endtabs %}
