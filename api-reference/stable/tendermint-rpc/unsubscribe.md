---
description: >-
  Example code for the unsubscribe JSON-RPC method. Complete guide on how to use
  unsubscribe JSON-RPC in GetBlock Web3 documentation.
---

# unsubscribe - Stable

Cancels a specific subscription on the WebSocket connection, identified by its event query.

## Method

```
unsubscribe
```

## Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| query     | string | The event query to unsubscribe from |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over WebSocket (append /websocket to the endpoint)
wscat -c "wss://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/websocket" \
  -x '{"jsonrpc": "2.0", "id": "getblock.io", "method": "unsubscribe", "params": {"query": "tm.event='NewBlock'"}}'
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

| Field  | Type   | Description      |
| ------ | ------ | ---------------- |
| result | object | Empty on success |

## Use Cases

* **Cleanup**: Stop a specific event stream

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
