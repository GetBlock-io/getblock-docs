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
{% tab title="WebSocket (JavaScript)" %}
{% code title="subscribe.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  // Subscribe to new block headers
  const subscribeRequest = {
    jsonrpc: '2.0',
    id: 1,
    method: 'eth_subscribe',
    params: ['newHeads']
  };
  ws.send(JSON.stringify(subscribeRequest));
});

ws.on('message', (data) => {
  const response = JSON.parse(data);
  console.log('Received:', response);
});
```
{% endcode %}
{% endtab %}

{% tab title="WebSocket (Python)" %}
{% code title="subscribe.py" %}
```python
import asyncio
import websockets
import json

async def subscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as websocket:
        # Subscribe to new block headers
        request = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "eth_subscribe",
            "params": ["newHeads"]
        }
        await websocket.send(json.dumps(request))
        
        while True:
            response = await websocket.recv()
            print(json.loads(response))

asyncio.run(subscribe())
```
{% endcode %}
{% endtab %}

{% tab title="Subscribe to Logs (JavaScript snippet)" %}
{% code title="subscribe-logs.js" %}
```javascript
const subscribeRequest = {
  jsonrpc: '2.0',
  id: 1,
  method: 'eth_subscribe',
  params: [
    'logs',
    {
      address: '0xContractAddress',
      topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
    }
  ]
};
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
