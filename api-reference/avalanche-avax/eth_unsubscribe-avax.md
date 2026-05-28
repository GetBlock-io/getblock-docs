---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Complete guide on how to
  use eth_unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_unsubscribe - AVAX

Cancels an existing subscription created with `eth_subscribe`. Like `eth_subscribe`, only available over WebSocket transport on the C-Chain.

## Parameters

| Parameter      | Type   | Required | Description                                     |
| -------------- | ------ | -------- | ----------------------------------------------- |
| subscriptionId | string | Yes      | The subscription ID returned by `eth_subscribe` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL (wscat)" overflow="wrap" %}
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

* Stopping a live feed when the user navigates away
* Reconnecting subscribers after a connection drop
* Cleanup in WebSocket-based microservices

## Error Handling

| Status Code | Error Message     | Cause                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                          |
| -32600      | Invalid Request   | `eth_unsubscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const result = await client.request({ method: 'eth_unsubscribe', params: ["0x9cef478923ff08bf67fde6c64013158d"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
