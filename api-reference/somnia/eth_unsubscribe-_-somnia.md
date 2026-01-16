---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Complete guide on how to
  use eth_unsubscribe JSON-RPC in GetBlock Web3 documentation
---

# eth\_unsubscribe \_ Somnia

This method cancels an existing subscription created with `eth_subscribe` on the Somnia network. This is important for resource management, especially when monitoring high-throughput events.

{% hint style="warning" %}
&#x20;This method requires a WebSocket connection, not HTTP.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                   |
| -------------- | ------ | -------- | ----------------------------- |
| subscriptionId | string | Yes      | The subscription ID to cancel |

## Request Example

{% tabs %}
{% tab title="WebSocket (JavaScript)" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  const unsubscribeRequest = {
    jsonrpc: '2.0',
    id: 1,
    method: 'eth_unsubscribe',
    params: ['0x9cef478923ff08bf67fde6c64013158d']
  };
  ws.send(JSON.stringify(unsubscribeRequest));
});

ws.on('message', (data) => {
  const response = JSON.parse(data);
  console.log('Unsubscribed:', response.result);
});
```
{% endtab %}

{% tab title="WebSocket (Python)" %}
```python
import asyncio
import websockets
import json

async def unsubscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as websocket:
        request = {
            "jsonrpc": "2.0",
            "id": 1,
            "method": "eth_unsubscribe",
            "params": ["0x9cef478923ff08bf67fde6c64013158d"]
        }
        await websocket.send(json.dumps(request))
        response = await websocket.recv()
        print(json.loads(response))

asyncio.run(unsubscribe())
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                           |
| --------- | ------- | ------------------------------------- |
| result    | boolean | True if unsubscribed, false otherwise |

## Use Cases

* Clean up subscriptions when done
* Manage connection resources
* Switch between different subscriptions
* Handle application shutdown

## Error Handling

| Error Code   | Description             |
| ------------ | ----------------------- |
| -32602       | Invalid subscription ID |
| -32603       | Internal error          |
| false result | Subscription not found  |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.WebSocketProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');

// Subscribe and store unsubscribe function
const listener = (blockNumber) => console.log('Block:', blockNumber);
provider.on('block', listener);

// Later, unsubscribe
provider.off('block', listener);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, webSocket } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: webSocket('wss://go.getblock.io/<ACCESS-TOKEN>/')
});

const unwatch = client.watchBlocks({
  onBlock: (block) => console.log(block)
});

// Later, unsubscribe
unwatch();
```
{% endtab %}
{% endtabs %}
