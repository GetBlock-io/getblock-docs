---
description: >-
  Example code for the genesis JSON-RPC method. Complete guide on how to use
  genesis JSON-RPC in GetBlock Web3 documentation.
---

# genesis - Stable

Returns the genesis document. For Stable the genesis is large and exceeds the response limit — use genesis\_chunked instead.

## Method

```
genesis
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis", "params": {}}'

# URI over HTTP (GET)
curl "${STABLE_TM}genesis"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "genesis": {
            "chain_id": "stablechain_988-1",
            "initial_height": "1",
            "consensus_params": {},
            "app_hash": ""
        }
    }
}
```

## Response Fields

| Field   | Type   | Description                                                                   |
| ------- | ------ | ----------------------------------------------------------------------------- |
| genesis | object | The genesis document (chain\_id, initial height, consensus params, app state) |

## Use Cases

* **Chain bootstrap**: Read genesis parameters

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
