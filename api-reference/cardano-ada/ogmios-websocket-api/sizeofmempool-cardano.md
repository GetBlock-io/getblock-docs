---
description: >-
  Example code for the sizeOfMempool WebSocket method. Complete guide on how to
  use sizeOfMempool WebSocket in GetBlock Web3 documentation.
---

# sizeOfMempool - Cardano

This method returns the current size and capacity of the acquired mempool snapshot, including the number of transactions and the used and maximum byte and execution-unit budgets. This protocol requires a WebSocket connection.

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
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "sizeOfMempool", "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "sizeOfMempool", "id": "getblock.io"}))
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
    "method": "sizeOfMempool",
    "result": {
        "maxCapacity": {
            "bytes": 4194304
        },
        "currentSize": {
            "bytes": 892014
        },
        "transactions": {
            "count": 43
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| maxCapacity  | object | Maximum mempool capacity in bytes      |
| currentSize  | object | Current used size in bytes             |
| transactions | object | Number of transactions in the snapshot |

## Use Cases

* **Congestion Signals**: Gauge how full the mempool is
* **Backpressure**: Delay submission when the mempool is near capacity
* **Dashboards**: Display mempool depth and capacity
* **Capacity Planning**: Track mempool pressure over time

## Error Handling

| Error Code | Message              | Description                                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| -32602     | Invalid params       | The request is malformed for the current protocol state |
| 4000       | Must acquire mempool | A snapshot must be acquired before this request         |
| -32603     | Internal error       | The node failed to answer the mempool request           |
