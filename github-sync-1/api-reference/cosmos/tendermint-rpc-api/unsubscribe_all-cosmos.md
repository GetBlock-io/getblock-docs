---
description: >-
  Example code for the unsubscribe_all JSON-RPC method. Complete guide on how to
  use unsubscribe_all JSON-RPC in GetBlock Web3 documentation.
---

# unsubscribe\_all - Cosmos

Cancels all subscriptions on the current WebSocket connection.

## Method

```
unsubscribe_all
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over WebSocket (append /websocket to the endpoint)
wscat -c "wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket" \
  -x '{"jsonrpc": "2.0", "id": "getblock.io", "method": "unsubscribe_all", "params": {}}'
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

* **Teardown**: Drop all streams before closing a connection

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
