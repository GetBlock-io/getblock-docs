---
description: >-
  Example code for the acquireLedgerState WebSocket method. Complete guide on
  how to use acquireLedgerState WebSocket in GetBlock Web3 documentation.
---

# acquireLedgerState - Cardano

This method acquires a fixed ledger state at a given point, so that subsequent state queries are answered consistently against that point rather than a moving tip. This protocol requires a WebSocket connection.

## Parameters

| Parameter | Type             | Required | Description                                             |
| --------- | ---------------- | -------- | ------------------------------------------------------- |
| point     | string or object | No       | The point to acquire, or "origin" or "tip". Default tip |

## Request

{% tabs %}
{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import WebSocket from 'ws';

const client = new WebSocket('wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

client.once('open', () => {
    client.send(JSON.stringify({"jsonrpc": "2.0", "method": "acquireLedgerState", "params": {"point": {"slot": 123456789, "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"}}, "id": "getblock.io"}));
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
        await ws.send(json.dumps({"jsonrpc": "2.0", "method": "acquireLedgerState", "params": {"point": {"slot": 123456789, "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"}}, "id": "getblock.io"}))
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
    "method": "acquireLedgerState",
    "result": {
        "acquired": "ledgerState",
        "point": {
            "slot": 123456789,
            "id": "3e6f2d8c9a1b4e7f0c2d5a8b1e4f7a0c3d6b9e2f5a8c1d4e7b0f3a6c9d2e5b8f"
        }
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Field    | Type   | Description                                     |
| -------- | ------ | ----------------------------------------------- |
| acquired | string | Confirmation that the ledger state was acquired |
| point    | object | The point the ledger state was acquired at      |

## Use Cases

* **Consistent Snapshots**: Answer many queries against one fixed ledger point
* **Reconciliation**: Read balances and UTXOs at the same point
* **Historical Reads**: Acquire a recent past point within the safe zone
* **Analytics**: Pin a batch of queries to one state

## Error Handling

| Error Code | Message         | Description                                                          |
| ---------- | --------------- | -------------------------------------------------------------------- |
| -32602     | Invalid params  | The requested point is missing or malformed                          |
| 2000       | Acquire failure | The requested point is no longer on the chain and cannot be acquired |
| -32603     | Internal error  | The node failed to acquire the requested state                       |
