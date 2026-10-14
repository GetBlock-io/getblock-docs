---
description: >-
  Example code for the spec JSON-RPC method. Complete guide on how to use spec
  JSON-RPC in GetBlock Web3 documentation.
---

# spec - Movement

Returns the node's own OpenAPI specification. Useful for tooling that wants the machine-readable schema straight from the node.

## Endpoint

```http
GET /v1/spec
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/spec"
```
{% endcode %}

## Response

```json
{
    "openapi": "3.0.0",
    "info": {
        "title": "Aptos Node API",
        "version": "1.0.0"
    },
    "paths": {}
}
```

## Response Fields

| Field  | Type   | Description                 |
| ------ | ------ | --------------------------- |
| (root) | object | The node's OpenAPI document |

## Use Cases

* **Tooling**: Fetch the live OpenAPI schema from the node

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
