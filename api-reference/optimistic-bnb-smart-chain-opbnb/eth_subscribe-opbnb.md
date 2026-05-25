---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Сomplete guide on how to
  use eth_subscribe JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_subscribe - opBNB

This method creates a real-time subscription over a WebSocket connection. Available subscription types are `newHeads` (new block headers), `logs` (matching contract events), and `newPendingTransactions` (pending transactions).&#x20;

{% hint style="info" %}
This method is only available over WebSocket transport — use `wss://go.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                               |
| ---------------- | ------ | -------- | --------------------------------------------------------- |
| subscriptionType | string | Yes      | One of `"newHeads"`, `"logs"`, `"newPendingTransactions"` |
| filterObject     | object | No       | For `"logs"`: filter with `address` and/or `topics`       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL (wscat)" overflow="wrap" %}
```bash
# This method requires WebSocket connection
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/

> {"jsonrpc":"2.0","method":"eth_subscribe","params":["newheads"],"id":"getblock.io"}
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
        method: 'eth_subscribe',
        params: ['newHeads'],
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
            "method": "eth_subscribe",
            "params": ["newHeads"],
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
        "method": "eth_subscribe",
        "params": ["newHeads"],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```

## Response Parameters

| Field  | Type   | Description                                                                                            |
| ------ | ------ | ------------------------------------------------------------------------------------------------------ |
| result | string | Subscription ID, used to match incoming `eth_subscription` notifications and to call `eth_unsubscribe` |

## Use Cases

* Real-time wallet activity notifications
* Live DEX trade feeds for trading UIs
* Streaming indexers that react to new blocks instantly
* Mempool surveillance for MEV bots

## Error Handling

| Status Code | Error Message     | Cause                                                                    |
| ----------- | ----------------- | ------------------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                      |
| -32602      | Invalid params    | Request parameters are missing or malformed                              |
| -32601      | Method not found  | The method is not supported by this node                                 |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                        |
| -32600      | Invalid Request   | `eth_subscribe` was called over HTTP; use a WebSocket connection instead |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js (WebSocket)" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});
```
{% endtab %}

{% tab title="Viem (WebSocket)" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { opBNB } from 'viem/chains';

const client = createPublicClient({
    chain: opBNB,
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlockNumber({
    onBlockNumber: (blockNumber) => console.log('New block:', blockNumber)
});
```
{% endtab %}
{% endtabs %}
