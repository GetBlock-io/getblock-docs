---
description: >-
  Example code for the consensus_subscribe JSON-RPC method. Complete guide on
  how to use consensus_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_subscribe - Tempo

WebSocket subscription to consensus events. Emits events when blocks are notarized, finalized, or views are nullified. Available on validator nodes over WebSocket only.

{% hint style="warning" %}
**VALIDATOR-NODE-ONLY METHOD.** This method is part of the `consensus_*` namespace and is only available on Tempo validator nodes. It is **not** available on shared RPC infrastructure such as GetBlock's standard shared endpoints. Calling this method against a non-validator node returns `-32601 Method not found`. If you need access, contact GetBlock for dedicated validator node access.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "consensus_subscribe", "params": [], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "consensus_subscribe",
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
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "method": "consensus_subscribe",
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
    let (mut ws_stream, _) = connect_async("wss://go.getblock.io/<ACCESS-TOKEN>/").await?;

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "consensus_subscribe",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```
{% endcode %}

## Response Parameters

| Field               | Type    | Description                                                                                                                    |
| ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------ |
| result              | string  | Subscription ID. Streamed events arrive as separate WebSocket messages with `type` ∈ `notarized` \| `finalized` \| `nullified` |
| (event) type        | string  | One of `notarized`, `finalized`, `nullified`                                                                                   |
| (event) epoch       | integer | Consensus epoch                                                                                                                |
| (event) view        | integer | Consensus view                                                                                                                 |
| (event) digest      | string  | Block digest (omitted for `nullified`)                                                                                         |
| (event) certificate | string  | BLS certificate (omitted for `nullified`)                                                                                      |
| (event) block       | object  | Full Tempo block (omitted for `nullified`)                                                                                     |
| (event) seen        | integer | Unix timestamp in milliseconds when the node observed the event                                                                |

## Use Cases

* Validator-side real-time consensus monitoring
* Bridge relayers that act immediately on finalization
* Low-latency notarized-block feeds for advanced UX

## Error Handling

| Status Code | Error Message     | Cause                                                                            |
| ----------- | ----------------- | -------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                              |
| -32602      | Invalid params    | Request parameters are missing or malformed                                      |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                |
| -32601      | Method not found  | This is a validator-only WebSocket method — not exposed on shared infrastructure |
| -32600      | Invalid Request   | Called over HTTP instead of WebSocket                                            |

## SDK Integration

{% tabs %}
{% tab title="WebSocket (ws)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 'getblock.io',
        method: 'consensus_subscribe',
        params: []
    }));
});

ws.on('message', (data) => console.log(JSON.parse(data.toString())));
```
{% endtab %}

{% tab title="WebSocket (websockets)" %}
```python
import asyncio, json, websockets

async def main():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
            'jsonrpc': '2.0', 'id': 'getblock.io',
            'method': 'consensus_subscribe', 'params': []
        }))
        async for msg in ws:
            print(json.loads(msg))

asyncio.run(main())
```
{% endtab %}
{% endtabs %}
