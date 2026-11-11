---
description: >-
  Example code for the health JSON-RPC method. Complete guide on how to use
  health JSON-RPC in GetBlock Web3 documentation.
---

# health - TAC

Returns an empty result if the node is healthy; otherwise an error.

## Method

```
health
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "health", "params": {}}'

# URI over HTTP (GET)
curl "${TAC_TM}health"
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

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| result | object | Empty object when the node is healthy |

## Use Cases

* **Liveness probe**: Confirm the node is responsive

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
