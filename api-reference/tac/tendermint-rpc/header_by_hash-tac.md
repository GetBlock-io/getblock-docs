---
description: >-
  Example code for the header_by_hash JSON-RPC method. Complete guide on how to
  use header_by_hash JSON-RPC in GetBlock Web3 documentation.
---

# header\_by\_hash - TAC

Returns a block header by its hash.

## Method

```
header_by_hash
```

## Parameters

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| hash      | string | Block hash  |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "header_by_hash", "params": {"hash": "0xA1B2C3..."}}'

# URI over HTTP (GET)
curl "${TAC_TM}header_by_hash?hash=0xA1B2C3..."
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "header": {
            "height": "5123456"
        }
    }
}
```

## Response Fields

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| header | object | The block header for the given hash |

## Use Cases

* **Hash lookup**: Resolve a header when you have its block hash

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
