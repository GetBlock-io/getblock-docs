---
description: >-
  Example code for the pendingLogs Flashblocks method. Complete guide on how to
  use pendingLogs  Flashblocks in GetBlock Web3 documentation.
---

# pendingLogs - Flashblocks

This Subscribes to contract event logs emitted by preconfirmed transactions — sub-block latency for on-chain events. Filter by contract address (single or array) and event topics (same format as `eth_getLogs`).&#x20;

This is the fastest way to react to on-chain events, up to \~1.8 seconds ahead of the standard `logs` subscription.

{% hint style="warning" %}
**WebSocket-only method.** This method requires the WebSocket transport at `wss://go.getblock.io/<ACCESS-TOKEN>/`. It will not work via HTTP POST. Preconfirmed events arrive at the Flashblock cadence — approximately every 200ms on Base, 250ms on Optimism.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                                                                 |
| ---------------- | ------ | -------- | ------------------------------------------------------------------------------------------- |
| subscriptionType | string | Yes      | Must be `"pendingLogs"`                                                                     |
| filter           | object | No       | Filter object: `address` (single address or array), `topics` (same format as `eth_getLogs`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Use wscat (or similar) to connect first:
wscat -c 'wss://go.getblock.io/<ACCESS-TOKEN>/'

# Then send:
{"jsonrpc": "2.0", "method": "pendingLogs", "params": ["pendingLogs", {"address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}], "id": "getblock.io"}
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
    "method": "pendingLogs",
    "params": [
        "pendingLogs",
        {
            "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
    "id": "getblock.io"
}));
});

ws.on('message', (data) => {
    const msg = JSON.parse(data.toString());
    if (msg.method === 'eth_subscription') {
        // Preconfirmed data update
        console.log(msg.params.result);
    } else {
        // Subscription ID response
        console.log('Subscribed:', msg.result);
    }
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
    "method": "pendingLogs",
    "params": [
        "pendingLogs",
        {
            "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
            ]
        }
    ],
    "id": "getblock.io"
}))
        async for message in ws:
            msg = json.loads(message)
            if msg.get('method') == 'eth_subscription':
                # Preconfirmed data update
                print(msg['params']['result'])
            else:
                # Subscription ID response
                print('Subscribed:', msg.get('result'))

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
        "method": "pendingLogs",
        "params": [
                "pendingLogs",
                {
                        "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                        "topics": [
                                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                        ]
                }
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
    "result": "0x7e2ef8b9c0d1e2f3a4b5c6d7e8f9a0b1"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                                                         |
| ------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| result | string | Hex-encoded subscription ID. Each subsequent notification carries a Log object matching the filter, from a preconfirmed transaction |

## Use Cases

* Real-time event indexers — the lowest-latency way to react to on-chain events
* Trading bots watching for specific ERC-20 transfers, DEX swaps, or oracle updates
* Liquidation bots watching for Aave/Compound account state changes
* Social/gaming applications reacting to on-chain events with instant UX feedback

## Error Handling

| Status Code | Error Message     | Cause                                                         |
| ----------- | ----------------- | ------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                           |
| -32602      | Invalid params    | Request parameters are missing or malformed                   |
| -32603      | Internal error    | Server-side error while processing the request                |
| 429         | Too Many Requests | Rate limit exceeded for your plan                             |
| -32600      | Invalid Request   | Subscription called over HTTP; use WebSocket                  |
| -32602      | Invalid filter    | Filter object malformed — check `address` and `topics` format |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Flashblocks subscription types aren't first-class in ethers — use the raw send interface:
const subscriptionId = await provider.send('eth_subscribe', ["pendingLogs", {"address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]);
console.log('Subscribed:', subscriptionId);

// Listen for incoming events on the WebSocket transport
provider.websocket.addEventListener('message', (event) => {
    const msg = JSON.parse(event.data);
    if (msg.method === 'eth_subscription' && msg.params?.subscription === subscriptionId) {
        console.log('Flashblocks update:', msg.params.result);
    }
});
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, webSocket } from 'viem';

const client = createPublicClient({
    transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

// Flashblocks subscription types use the raw request interface:
const subscriptionId = await client.request({
    method: 'eth_subscribe',
    params: ["pendingLogs", {"address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}]
});
console.log('Subscribed:', subscriptionId);
```
{% endcode %}
{% endtab %}
{% endtabs %}
