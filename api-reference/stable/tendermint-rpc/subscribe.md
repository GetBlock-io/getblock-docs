---
description: >-
  Example code for the subscribe JSON-RPC method. Complete guide on how to use
  subscribe JSON-RPC in GetBlock Web3 documentation.
---

# subscribe - Stable

Subscribes to events matching a Tendermint event query over the WebSocket endpoint. Matching events, such as new blocks and transactions, are streamed to the connection.

## Method

```
subscribe
```

## Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| query     | string | Tendermint event query, e.g. `"tm.event='NewBlock'"` |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over WebSocket (append /websocket to the endpoint)
wscat -c "wss://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/websocket" \
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
