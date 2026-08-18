---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_subscribe - Celo

This method creates a subscription for real-time event notifications on the Celo network.&#x20;

{% hint style="info" %}
This requires a WebSocket connection and is ideal for tracking new blocks, pending transactions, or contract events with Celo's fast \~1 second block times.
{% endhint %}

## Parameters

| Parameter        | Type   | Required | Description                                        |
| ---------------- | ------ | -------- | -------------------------------------------------- |
| subscriptionType | string | Yes      | Type: "newHeads", "logs", "newPendingTransactions" |
| options          | object | No       | Filter options (for "logs" type)                   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL (wscat)" overflow="wrap" %}
```bash
# This method requires WebSocket connection
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

> {"jsonrpc":"2.0","method":"eth_subscribe","params":["newheads"],"id":"getblock.io"}
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
{% code title="JavaScript (ws)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

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
    uri = "wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"
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
    let url = "wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/";
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

{% tabs %}
{% tab title="Result" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```
{% endtab %}

{% tab title="Subscription Notification" %}
```json
{
  "jsonrpc": "2.0",
  "method": "eth_subscription",
  "params": {
    "subscription": "0x9cef478923ff08bf67fde6c64013158d",
    "result": {
      "number": "0x1d9f2a8",
      "hash": "0x4e3a375...",
      "parentHash": "0xb903239...",
      "timestamp": "0x65a1b2c3"
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Response Definition

| Field  | Type   | Description     |
| ------ | ------ | --------------- |
| result | string | Subscription ID |

## Use Cases

* Real-time block notifications
* Live stablecoin transfer tracking
* Pending transaction monitoring
* DeFi event streaming

## Error Handling

| Error Code | Description               |
| ---------- | ------------------------- |
| -32602     | Invalid subscription type |
| -32603     | Internal error            |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-subscribe.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.WebSocketProvider('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
  console.log('New block:', blockNumber);
});
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-watchBlocks.js" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { celo } from 'viem/chains';

const client = createPublicClient({
  chain: celo,
  transport: webSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
  onBlock: block => console.log('Block:', block.number)
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
