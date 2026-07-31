---
description: >-
  Example code for the hasTransaction WebSocket method. Complete guide on how to
  use hasTransaction WebSocket in GetBlock Web3 documentation.
---

# hasTransaction - Cardano

This method reports whether a transaction with the given id is present in the acquired mempool snapshot. This protocol requires a WebSocket connection.

## Parameters

| Parameter | Type   | Required | Description                    |
| --------- | ------ | -------- | ------------------------------ |
| id        | string | Yes      | The transaction id to look for |

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://go.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "hasTransaction", "params": {"id": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}, "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "hasTransaction", "params": {"id": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642"}, "id": "getblock.io"}))
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
    "method": "hasTransaction",
    "result": true,
    "id": "getblock.io"
}
```

## Response Parameters

| Field  | Type    | Description                                                 |
| ------ | ------- | ----------------------------------------------------------- |
| result | boolean | true if the transaction is in the snapshot, otherwise false |

## Use Cases

* **Propagation Confirmation**: Confirm a submitted transaction is pending
* **Deduplication**: Avoid resubmitting a transaction already in the mempool
* **Status Tracking**: Report a transaction as pending to a user
* **Monitoring**: Watch for a specific transaction to appear

## Error Handling

| Error Code | Message              | Description                                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| -32602     | Invalid params       | The request is malformed for the current protocol state |
| 4000       | Must acquire mempool | A snapshot must be acquired before this request         |
| -32603     | Internal error       | The node failed to answer the mempool request           |
