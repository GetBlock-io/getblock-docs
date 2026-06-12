---
description: >-
  Example code for the author_submitAndWatchExtrinsic JSON-RPC method. Complete
  guide on how to use author_submitAndWatchExtrinsic JSON-RPC in GetBlock Web3
  documentation.
---

# author\_submitAndWatchExtrinsic - Bittensor

Submits a signed extrinsic and watches it through inclusion and finalization. Returns a subscription ID; status updates (`ready`, `inBlock`, `finalized`, etc.) arrive as separate WebSocket messages.

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

{% hint style="warning" %}
**WebSocket-only method.** This method requires the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/`. It will not work via HTTP POST.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| extrinsic | string | Yes      | Hex-encoded SCALE-serialized signed extrinsic |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "author_submitAndWatchExtrinsic", "params": ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."], "id": "getblock.io"}
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
    "method": "author_submitAndWatchExtrinsic",
    "params": [
        "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
    "method": "author_submitAndWatchExtrinsic",
    "params": [
        "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
        "method": "author_submitAndWatchExtrinsic",
        "params": [
                "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
    "result": "B5cD6eF7gH8iJ9kL0m"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                       |
| ------ | ------ | ----------------------------------------------------------------- |
| result | string | Subscription ID — incoming notifications carry status transitions |

## Use Cases

* Wallet UIs showing real-time transaction progress (pending → inBlock → finalized)
* Server-side relayers that need to confirm extrinsic outcomes
* Replacing inefficient polling for receipts

## Error Handling

| Status Code | Error Message     | Cause                                                                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found  | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error    | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                                                          |

## SDK Integration

{% tabs %}
{% tab title="Polkadot.js (TypeScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Typed wrapper: api.rpc.author.submitAndWatchExtrinsic(...)
const result = await api.rpc.author.submitAndWatchExtrinsic("0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4...");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'author_submitAndWatchExtrinsic', params: ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("author_submitAndWatchExtrinsic", ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."])
print(result)
```
{% endtab %}
{% endtabs %}
