---
description: >-
  Example code for the subscribe JSON-RPC method. Complete guide on how to use
  subscribe JSON-RPC in GetBlock Web3 documentation.
---

# subscribe - Cosmos

Subscribes to events matching a Tendermint event query over the WebSocket endpoint. Matching events (new blocks, transactions) are then streamed to the connection.

## Method

```
subscribe
```

## Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| query     | string | Tendermint event query, e.g. "tm.event='NewBlock'" |

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over WebSocket (append /websocket to the endpoint)
wscat -c "wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket" \
  -x '{"jsonrpc": "2.0", "id": "getblock.io", "method": "subscribe", "params": {"query": "tm.event='NewBlock'"}}'
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {}
}
```

## Response Fields

| Field  | Type   | Description                                                       |
| ------ | ------ | ----------------------------------------------------------------- |
| result | object | Empty on subscribe; matching events arrive as subsequent messages |

## Use Cases

* **Live blocks**: Stream NewBlock events
* **Tx streams**: Subscribe to transactions matching a query

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
