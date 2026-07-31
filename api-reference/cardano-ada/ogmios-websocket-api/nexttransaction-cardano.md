---
description: >-
  Example code for the nextTransaction WebSocket method. Complete guide on how
  to use nextTransaction WebSocket in GetBlock Web3 documentation.
---

# nextTransaction - Cardano

This method returns the next transaction in the acquired mempool snapshot, allowing a client to list all pending transactions one at a time. This protocol requires a WebSocket connection.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| fields    | string | No       | Set to "all" to return full transaction objects instead of ids |

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "nextTransaction", "params": {"fields": "all"}, "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "nextTransaction", "params": {"fields": "all"}, "id": "getblock.io"}))
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
    "method": "nextTransaction",
    "result": {
        "transaction": {
            "id": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field       | Type   | Description                                                          |
| ----------- | ------ | -------------------------------------------------------------------- |
| transaction | object | The next mempool transaction, or null when the snapshot is exhausted |

## Use Cases

* **Mempool Listing**: Enumerate all pending transactions in a snapshot
* **Payment Detection**: Detect an incoming transaction before confirmation
* **Fee Analysis**: Collect pending transactions for fee modeling
* **Monitoring**: Feed pending transactions into an alerting pipeline

## Error Handling

| Error Code | Message              | Description                                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| -32602     | Invalid params       | The request is malformed for the current protocol state |
| 4000       | Must acquire mempool | A snapshot must be acquired before this request         |
| -32603     | Internal error       | The node failed to answer the mempool request           |
