---
description: >-
  Example code for the releaseLedgerState WebSocket method. Complete guide on
  how to use releaseLedgerState WebSocket in GetBlock Web3 documentation.
---

# releaseLedgerState - Cardano

This method releases a previously acquired ledger state, freeing the node to serve queries against the moving tip again. This protocol requires a WebSocket connection.

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
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "releaseLedgerState", "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "releaseLedgerState", "id": "getblock.io"}))
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
    "method": "releaseLedgerState",
    "result": {
        "released": "ledgerState"
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type   | Description                                     |
| -------- | ------ | ----------------------------------------------- |
| released | string | Confirmation that the ledger state was released |

## Use Cases

* **Resource Cleanup**: Release a snapshot when a query batch completes
* **Connection Hygiene**: Free held state before acquiring a new point
* **Long Sessions**: Cycle acquisitions over a persistent connection
* **Error Recovery**: Reset state after a failed query sequence

## Error Handling

| Error Code | Message         | Description                                                          |
| ---------- | --------------- | -------------------------------------------------------------------- |
| -32602     | Invalid params  | The requested point is missing or malformed                          |
| 2000       | Acquire failure | The requested point is no longer on the chain and cannot be acquired |
| -32603     | Internal error  | The node failed to acquire the requested state                       |
