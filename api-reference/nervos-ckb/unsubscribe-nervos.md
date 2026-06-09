---
description: >-
  Example code for the unsubscribe JSON-RPC method. Complete guide on how to use
  unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# unsubscribe - Nervos

Cancels an existing subscription. Pass the subscription ID returned by `subscribe`.

{% hint style="info" %}
**WebSocket-only method.** This method is part of the Subscription module and is only available over the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/`. It will not work via HTTP POST.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                             |
| ---------------- | ------ | -------- | --------------------------------------- |
| subscription\_id | string | Yes      | Subscription ID returned by `subscribe` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "unsubscribe", "params": ["0xf1"], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "unsubscribe",
    "params": [
        "0xf1"
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
    "method": "unsubscribe",
    "params": [
        "0xf1"
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
        "method": "unsubscribe",
        "params": [
                "0xf1"
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
    "result": true
}
```
{% endcode %}

## Response Parameters

| Field  | Type    | Description                                                                       |
| ------ | ------- | --------------------------------------------------------------------------------- |
| result | boolean | `true` if the subscription was cancelled, `false` if no such subscription existed |

## Use Cases

* Stopping a live feed when the user navigates away
* Cleanup in WebSocket-based microservices

## Error Handling

| Status Code | Error Message     | Cause                                                                  |
| ----------- | ----------------- | ---------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                    |
| -32602      | Invalid params    | Request parameters are missing or malformed                            |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                   |
| -32603      | Internal error    | Server-side error while processing the request                         |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                      |
| -32600      | Invalid Request   | `unsubscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos / ws" %}
```javascript
// Lumos doesn't expose CKB subscription methods directly. Use a raw
// WebSocket as shown in the Request Example above, or use the @ckb-ccc/core
// subscription helpers if available.
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');
ws.on('open', () => {
    ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 'sub-1',
        method: 'unsubscribe',
        params: ["0xf1"]
    }));
});
ws.on('message', (data) => console.log(JSON.parse(data.toString())));
```
{% endtab %}

{% tab title="websockets (Python)" %}
```python
# CKB SDK Python doesn't expose subscriptions directly — use websockets.
import asyncio, json, websockets

async def main():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
            'jsonrpc': '2.0', 'id': 'sub-1',
            'method': 'unsubscribe', 'params': ["0xf1"]
        }))
        async for msg in ws:
            print(json.loads(msg))

asyncio.run(main())
```
{% endtab %}
{% endtabs %}
