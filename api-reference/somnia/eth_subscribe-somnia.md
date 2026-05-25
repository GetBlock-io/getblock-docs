---
description: >-
  Example code for the eth_subscribe JSON-RPC method. Complete guide on how to
  use eth_subscribe JSON-RPC in GetBlock Web3 documentation
---

# eth\_subscribe - Somnia

This method creates a subscription over WebSocket on the Somnia network for real-time event notifications. Given Somnia's \~100ms block times and high throughput, subscriptions enable efficient real-time monitoring without polling. This method requires a WebSocket connection.

{% hint style="warning" %}
This method requires a WebSocket connection (not HTTP).
{% endhint %}

### Parameters

| Parameter        | Type   | Required | Description                                        |
| ---------------- | ------ | -------- | -------------------------------------------------- |
| subscriptionType | string | Yes      | Type: "newHeads", "logs", "newPendingTransactions" |
| filterObject     | object | No       | Filter options (for "logs" type)                   |

#### Subscription Types

* `newHeads` - Subscribe to new block headers
* `logs` - Subscribe to contract event logs
* `newPendingTransactions` - Subscribe to pending transactions

### Request Example

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

### Response Example

{% tabs %}
{% tab title="Subscription Confirmation" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x9cef478923ff08bf67fde6c64013158d"
}
```
{% endtab %}

{% tab title="New Block Notification" %}
{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "method": "eth_subscription",
  "params": {
    "subscription": "0x9cef478923ff08bf67fde6c64013158d",
    "result": {
      "number": "0x1a2b3c",
      "hash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
      "parentHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
      "timestamp": "0x65a1b2c3"
    }
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Parameters

| Parameter    | Type   | Description                              |
| ------------ | ------ | ---------------------------------------- |
| subscription | string | Subscription ID                          |
| result       | object | Event data (varies by subscription type) |

### Use Cases

* Real-time block monitoring
* Live event tracking for dApps
* Instant transaction notifications
* Building live dashboards
* MEV and arbitrage detection

### Error Handling

| Error Code        | Description                         |
| ----------------- | ----------------------------------- |
| -32602            | Invalid subscription type or filter |
| -32603            | Internal error                      |
| Connection closed | WebSocket disconnected              |

### SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
  console.log('New block:', blockNumber);
});

// Subscribe to contract events
const contract = new ethers.Contract(address, abi, provider);
contract.on('Transfer', (from, to, value) => {
  console.log('Transfer:', from, to, value);
});
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
  onBlock: (block) => console.log('New block:', block.number)
});

// Later: unwatch();
```
{% endcode %}
{% endtab %}
{% endtabs %}
