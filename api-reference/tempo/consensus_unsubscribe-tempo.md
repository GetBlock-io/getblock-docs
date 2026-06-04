---
description: >-
  Example code for the consensus_unsubscribe JSON-RPC method. Complete guide on
  how to use consensus_unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_unsubscribe - Tempo

{% hint style="warning" %}
**VALIDATOR-NODE-ONLY METHOD.** This method is part of the `consensus_*` namespace and is only available on Tempo validator nodes. It is **not** available on shared RPC infrastructure such as GetBlock's standard shared endpoints. Calling this method against a non-validator node returns `-32601 Method not found`. If you need access, contact GetBlock about dedicated validator-node access.
{% endhint %}

Cancels an existing consensus subscription created with `consensus_subscribe`. WebSocket-only and validator-only.

## Parameters

| Parameter      | Type   | Required | Description                                       |
| -------------- | ------ | -------- | ------------------------------------------------- |
| subscriptionId | string | Yes      | Subscription ID returned by `consensus_subscribe` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "consensus_unsubscribe", "params": ["0x9cef478923ff08bf67fde6c64013158d"], "id": "getblock.io"}
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "consensus_unsubscribe",
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
    "method": "consensus_unsubscribe",
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
        "method": "consensus_unsubscribe",
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

| Field  | Type    | Description                                                           |
| ------ | ------- | --------------------------------------------------------------------- |
| result | boolean | `true` if the subscription was cancelled, `false` if it did not exist |

## Use Cases

* Cleanup in long-running consensus-monitoring services
* Resetting subscriptions after a relayer pivots roles

## Error Handling

| Status Code | Error Message     | Cause                                                        |
| ----------- | ----------------- | ------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                          |
| -32602      | Invalid params    | Request parameters are missing or malformed                  |
| -32601      | Method not found  | Method does not exist or is not enabled on this node         |
| 429         | Too Many Requests | Rate limit exceeded for your plan                            |
| -32601      | Method not found  | Validator-only method — not exposed on shared infrastructure |

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
        method: 'consensus_unsubscribe',
        params: ["0x9cef478923ff08bf67fde6c64013158d"]
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
            'method': 'consensus_unsubscribe', 'params': ["0x9cef478923ff08bf67fde6c64013158d"]
        }))
        async for msg in ws:
            print(json.loads(msg))

asyncio.run(main())
```
{% endtab %}
{% endtabs %}
