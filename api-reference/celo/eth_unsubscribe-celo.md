---
description: >-
  Example code for the eth_unsubscribe JSON-RPC method. Complete guide on how to
  use eth_unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# eth\_unsubscribe - Celo

This method cancels an existing subscription on the Celo network. This should be called to clean up subscriptions when they are no longer needed.

### Parameters

| Parameter      | Type   | Required | Description               |
| -------------- | ------ | -------- | ------------------------- |
| subscriptionId | string | Yes      | Subscription ID to cancel |

### Request Example

{% tabs %}
{% tab title="WebSocket" %}
{% code title="websocket.js" %}
```javascript
const WebSocket = require('ws');

const ws = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'eth_unsubscribe',
    params: ['0x9cef478923ff08bf67fde6c64013158d']
  }));
});

ws.on('message', (data) => {
  console.log(JSON.parse(data));
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="unsubscribe.py" %}
```python
import asyncio
import websockets
import json

async def unsubscribe():
    uri = "wss://go.getblock.io/<ACCESS-TOKEN>/"
    async with websockets.connect(uri) as ws:
        await ws.send(json.dumps({
            "jsonrpc": "2.0",
            "id": 1,
            "method": "eth_unsubscribe",
            "params": ["0x9cef478923ff08bf67fde6c64013158d"]
        }))
        
        response = await ws.recv()
        print(json.loads(response))

asyncio.run(unsubscribe())
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": true
}
```
{% endcode %}

### Response Definition

| Field  | Type    | Description                         |
| ------ | ------- | ----------------------------------- |
| result | boolean | `true` if successfully unsubscribed |

### Use Cases

* Clean up unused subscriptions
* Manage connection resources
* Implement proper lifecycle management

### Error Handling

| Error Code | Description             |
| ---------- | ----------------------- |
| -32602     | Invalid subscription ID |
| -32603     | Internal error          |
