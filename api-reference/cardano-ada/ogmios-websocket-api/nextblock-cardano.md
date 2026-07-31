---
description: >-
  Example code for the nextBlock WebSocket method. Complete guide on how to use
  nextBlock WebSocket in GetBlock Web3 documentation.
---

# nextBlock - Cardano

This method requests the next chain-synchronization instruction. The node responds with either a forward roll to the next block or a backward roll to a previous point after a reorganization. This protocol requires a WebSocket connection.

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
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "nextBlock", "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "nextBlock", "id": "getblock.io"}))
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
    "method": "nextBlock",
    "result": {
        "direction": "forward",
        "block": {
            "era": "conway",
            "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f",
            "slot": 123456789,
            "height": 10453789
        },
        "tip": {
            "slot": 123456799,
            "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field     | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| direction | string | forward for a new block, or backward for a rollback    |
| block     | object | The block rolled forward to, present on a forward roll |
| point     | object | The point rolled back to, present on a backward roll   |
| tip       | object | The node's current tip                                 |

## Use Cases

* **Chain Indexing**: Stream blocks sequentially into an off-chain store
* **Reorg Handling**: React to backward rolls by rewinding local state
* **Event Pipelines**: Drive downstream processing from each new block
* **Explorers**: Ingest the chain block by block

## Error Handling

| Error Code | Message                | Description                                         |
| ---------- | ---------------------- | --------------------------------------------------- |
| -32602     | Invalid params         | The supplied points are missing or malformed        |
| 1000       | Intersection not found | None of the supplied points are on the node's chain |
| -32603     | Internal error         | The node failed to advance the chain-sync protocol  |
