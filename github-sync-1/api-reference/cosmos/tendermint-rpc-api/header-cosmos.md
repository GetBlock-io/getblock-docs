---
description: >-
  Example code for the header JSON-RPC method. Complete guide on how to use
  header JSON-RPC in GetBlock Web3 documentation.
---

# header - Cosmos

Returns the block header at a given height.

## Method

```
header
```

## Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| height    | string | Block height (omit for latest) |

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${COSMOS_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "header", "params": {"height": "31378844"}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}header?height=31378844"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "header": {
            "height": "31378844",
            "time": "2026-06-01T10:31:22Z",
            "chain_id": "cosmoshub-4",
            "app_hash": "F884..."
        }
    }
}
```

## Response Fields

| Field  | Type   | Description                                           |
| ------ | ------ | ----------------------------------------------------- |
| header | object | The block header: height, time, chain\_id, and hashes |

## Use Cases

* **Header read**: Fetch a header without the block body

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
