---
description: >-
  Example code for the releaseMempool WebSocket method. Complete guide on how to
  use releaseMempool WebSocket in GetBlock Web3 documentation.
---

# releaseMempool - Cardano

This method releases the acquired mempool snapshot, freeing the node to update its mempool view. This protocol requires a WebSocket connection.

## Parameters

This method does not require parameters.

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "releaseMempool", "id": "getblock.io"}));
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
    async with websockets.connect('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') as ws:
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "releaseMempool", "id": "getblock.io"}))
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
    "method": "releaseMempool",
    "result": {
        "released": "mempool"
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type   | Description                                         |
| -------- | ------ | --------------------------------------------------- |
| released | string | Confirmation that the mempool snapshot was released |

## Use Cases

* **Resource Cleanup**: Release a snapshot when inspection completes
* **Snapshot Cycling**: Release before acquiring a fresh snapshot
* **Long Sessions**: Manage mempool snapshots over a persistent connection
* **Error Recovery**: Reset mempool state after a failed query sequence

## Error Handling

| Error Code | Message              | Description                                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| -32602     | Invalid params       | The request is malformed for the current protocol state |
| 4000       | Must acquire mempool | A snapshot must be acquired before this request         |
| -32603     | Internal error       | The node failed to answer the mempool request           |
