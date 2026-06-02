---
description: >-
  Example code for the unsubscribe_all JSON-RPC method. Complete guide on how to
  use unsubscribe_all JSON-RPC in GetBlock Web3 documentation.
---

# unsubscribe\_all - Cosmos

Cancels all active subscriptions on the current WebSocket connection. Useful when resetting the subscription state or before cleanly disconnecting.

{% hint style="info" %}
**WebSocket-only method.** This method is only available over the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/websocket`. It will not work via HTTP POST. Use a WebSocket client such as wscat, the @cosmjs/tendermint-rpc WebSocketClient, or Python's websockets library.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/websocket'

# Then send:
{"jsonrpc": "2.0", "method": "unsubscribe_all", "params": [], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/websocket');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "unsubscribe_all",
    "params": [],
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
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/websocket') as ws:
        await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "method": "unsubscribe_all",
    "params": [],
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
    let (mut ws_stream, _) = connect_async("wss://go.getblock.io/<ACCESS-TOKEN>/websocket").await?;

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "unsubscribe_all",
        "params": [],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {}
}
```

## Response Parameters

| Field  | Type   | Description             |
| ------ | ------ | ----------------------- |
| result | object | Empty object on success |

## Use Cases

* Bulk-clearing subscriptions before disconnecting
* Resetting state in long-lived WebSocket clients

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, WebsocketClient } from '@cosmjs/tendermint-rpc';

const wsClient = new WebsocketClient('wss://go.getblock.io/<ACCESS-TOKEN>/websocket');
const client = await Tendermint37Client.create(wsClient);

// Use the typed subscription methods on the client where available.
// For raw subscription access:
const stream = wsClient.listen({"jsonrpc": "2.0", "id": "sub-1", "method": "unsubscribe_all", "params": []});
stream.subscribe({
    next: (event) => console.log(event),
    error: (err) => console.error(err)
});
```
{% endtab %}

{% tab title="cosmpy / websockets" %}
```python
# cosmpy does not expose CometBFT WebSocket subscriptions directly.
# Use the `websockets` library as shown in the Request Example above.
import asyncio
import json
import websockets

async def stream():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/websocket') as ws:
        await ws.send(json.dumps({'jsonrpc': '2.0', 'id': 'sub-1', 'method': 'unsubscribe_all', 'params': []}))
        async for msg in ws:
            print(json.loads(msg))

asyncio.run(stream())
```
{% endtab %}
{% endtabs %}
