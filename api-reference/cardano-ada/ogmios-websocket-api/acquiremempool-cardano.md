---
description: >-
  Example code for the acquireMempool WebSocket method. Complete guide on how to
  use acquireMempool WebSocket in GetBlock Web3 documentation.
---

# acquireMempool - Cardano

This method acquires a snapshot of the node's mempool. Subsequent mempool queries are answered against this snapshot until it is released or re-acquired. This protocol requires a WebSocket connection.

## Parameters

This method does not require parameters.

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "acquireMempool", "id": "getblock.io"}));
});

client.on('message', (msg) => {
    console.log(JSON.parse(msg.toString()));
    client.close();
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import asyncio
import json
import websockets

async def main():
    async with websockets.connect('wss://go.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "acquireMempool", "id": "getblock.io"}))
        print(json.loads(await ws.recv()))

asyncio.run(main())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "method": "acquireMempool",
    "result": {
        "acquired": "mempool",
        "slot": 123456789
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type    | Description                                       |
| -------- | ------- | ------------------------------------------------- |
| acquired | string  | Confirmation that a mempool snapshot was acquired |
| slot     | integer | The slot at which the snapshot was taken          |

## Use Cases

* **Mempool Inspection**: Take a stable snapshot before listing transactions
* **Fee Monitoring**: Assess pending transactions at a fixed point
* **Propagation Checks**: Confirm a submitted transaction reached the mempool
* **Analytics**: Sample mempool contents over time

## Error Handling

| Error Code | Message              | Description                                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| -32602     | Invalid params       | The request is malformed for the current protocol state |
| 4000       | Must acquire mempool | A snapshot must be acquired before this request         |
| -32603     | Internal error       | The node failed to answer the mempool request           |
