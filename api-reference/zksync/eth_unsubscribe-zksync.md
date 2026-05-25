---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Сomplete guide on how to
  use eth_unsubscribe JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - zkSync

Cancels an existing subscription created with `eth_subscribe`. Like `eth_subscribe`, only available over WebSocket transport.

## Parameters

| Parameter      | Type   | Required | Description                                     |
| -------------- | ------ | -------- | ----------------------------------------------- |
| subscriptionId | string | Yes      | The subscription ID returned by `eth_subscribe` |

## Request Example

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
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                          |
| -32600      | Invalid Request   | `eth_unsubscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="zksync2-python (Python)" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'eth_unsubscribe'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('eth_unsubscribe', ["0x9cef478923ff08bf67fde6c64013158d"])
print(result)
```
{% endtab %}
{% endtabs %}
