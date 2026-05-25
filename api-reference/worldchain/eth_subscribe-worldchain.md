---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - Worldchain

Creates a subscription on the World Chain network for particular events using WebSocket connection.

{% hint style="info" %}
This method can only be access via a Websocket
{% endhint %}

### Parameters

| Parameter        | Type   | Description                                                        |
| ---------------- | ------ | ------------------------------------------------------------------ |
| subscriptionType | string | Type of subscription: "newHeads", "logs", "newPendingTransactions" |
| filterObject     | object | (Optional) Filter options for logs subscription                    |

### Request

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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```

### Response Parameters

| Field  | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| result | string | Subscription ID for managing the subscription |

### Use Case

The `eth_subscribe` method on World Chain is typically used for:

* Real-time updates
* Block notifications
* Log streaming
* Pending transaction monitoring

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// WebSocket connection required
const subscription = await wsProvider.send('eth_subscribe', ['newHeads']);
console.log('Subscription ID:', subscription);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// WebSocket connection required
const unwatch = client.watchBlockNumber({
    onBlockNumber: (blockNumber) => console.log(blockNumber)
});
```
{% endtab %}
{% endtabs %}
