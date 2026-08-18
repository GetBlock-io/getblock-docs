---
description: >-
  Example code for the findIntersection  WebSocket method. Complete guide on how
  to use findIntersection WebSocket in GetBlock Web3 documentation.
---

# findIntersection - Cardano

This method finds the intersection between the node's chain and a set of candidate points supplied by the client. It is the first step of the chain-synchronization protocol and sets the position from which nextBlock streams. This protocol requires a WebSocket connection.

## Parameters

| Parameter | Type  | Required | Description                                              |
| --------- | ----- | -------- | -------------------------------------------------------- |
| points    | array | Yes      | Candidate points, each a slot and block id, newest first |

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "findIntersection", "params": {"points": [{"slot": 123456789, "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"}]}, "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "findIntersection", "params": {"points": [{"slot": 123456789, "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"}]}, "id": "getblock.io"}))
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
    "method": "findIntersection",
    "result": {
        "intersection": {
            "slot": 123456789,
            "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
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

| Field        | Type   | Description                                                    |
| ------------ | ------ | -------------------------------------------------------------- |
| intersection | object | The point found on both the client's list and the node's chain |
| tip          | object | The node's current tip at the time of intersection             |

## Use Cases

* **Sync Start**: Set the starting point for streaming blocks
* **Resumption**: Resume synchronization from the last processed point
* **Reorg Recovery**: Re-anchor after a rollback by offering recent points
* **Checkpointing**: Confirm a known point is still on the node's chain

## Error Handling

| Error Code | Message                | Description                                         |
| ---------- | ---------------------- | --------------------------------------------------- |
| -32602     | Invalid params         | The supplied points are missing or malformed        |
| 1000       | Intersection not found | None of the supplied points are on the node's chain |
| -32603     | Internal error         | The node failed to advance the chain-sync protocol  |
