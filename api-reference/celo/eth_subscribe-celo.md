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
{% tab title="JavaScript (newHeads)" %}
{% code title="subscribe-newHeads.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'eth_subscribe',
    params: ['newHeads']
  }));
});

ws.on('message', (data) => {
  console.log(JSON.parse(data));
});
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (logs - cUSD Transfers)" %}
{% code title="subscribe-logs-cusd.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'eth_subscribe',
    params: [
      'logs',
      {
        address: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
        topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef']
      }
    ]
  }));
});

ws.on('message', (data) => {
  console.log('cUSD Transfer:', JSON.parse(data));
});
```
{% endcode %}
{% endtab %}

{% tab title="Python (websockets)" %}
{% code title="subscribe_newHeads.py" %}
```python
import asyncio
import websockets
import json

async def subscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as ws:
        await ws.send(json.dumps({
            "jsonrpc": "2.0",
            "id": 1,
            "method": "eth_subscribe",
            "params": ["newHeads"]
        }))
        
        async for message in ws:
            print(json.loads(message))

asyncio.run(subscribe())
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

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

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
  transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
  onBlock: block => console.log('Block:', block.number)
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
