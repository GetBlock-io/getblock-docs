---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Complete guide to using
  the eth_unsubscribe JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_unsubscribe - Robinhood

Cancels an existing subscription by ID.

{% hint style="warning" %}
**WebSocket-only method**
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                 |
| -------------- | ------ | -------- | ------------------------------------------- |
| subscriptionId | string | Yes      | Subscription ID returned by `eth_subscribe` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only method. Use wscat (or similar) to connect first:
wscat -c 'wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "eth_unsubscribe", "params": ["0xcd0c3e8af590364c09d0fa6a1210faf5"], "id": "getblock.io"}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import WebSocket from 'ws';

const ws = new WebSocket('wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
    ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0xcd0c3e8af590364c09d0fa6a1210faf5"
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
{% code overflow="wrap" %}
```python
import asyncio
import json
import websockets

async def main():
    async with websockets.connect('wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_unsubscribe",
    "params": [
        "0xcd0c3e8af590364c09d0fa6a1210faf5"
    ],
    "id": "getblock.io"
}))
        async for message in ws:
            print(json.loads(message))

asyncio.run(main())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (Reqwest)" %}
{% code overflow="wrap" %}
```rust
use tokio_tungstenite::connect_async;
use tokio_tungstenite::tungstenite::Message;
use serde_json::json;
use futures_util::{SinkExt, StreamExt};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let (mut ws_stream, _) = connect_async("wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/").await?;

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_unsubscribe",
        "params": [
                "0xcd0c3e8af590364c09d0fa6a1210faf5"
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

## Response Parameters

| Field  | Type    | Description                                          |
| ------ | ------- | ---------------------------------------------------- |
| result | boolean | `true` if cancelled, `false` if no such subscription |

## Use Cases

* Cleanup when a live feed is no longer needed
* Connection lifecycle management

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// For 'newHeads' subscription:
provider.on('block', (blockNumber) => {
    console.log('New block:', blockNumber);
});

// For 'logs' subscription with a filter:
// provider.on({ address: '0x...', topics: [...] }, (log) => { ... });
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';

const client = createPublicClient({
    transport: webSocket('wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Subscribe to new blocks:
const unwatch = client.watchBlocks({
    onBlock: block => console.log('New block:', block.number)
});
// Call unwatch() when done.
```
{% endtab %}
{% endtabs %}
