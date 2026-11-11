---
description: >-
  Example code for the genesis JSON-RPC method. Complete guide on how to use
  genesis JSON-RPC in GetBlock Web3 documentation.
---

# genesis - TAC

Returns the genesis document. For large chains the genesis may exceed the response limit — use genesis\_chunked instead.

## Method

```
genesis
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis", "params": {}}'

# URI over HTTP (GET)
curl "${TAC_TM}genesis"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "genesis": {
            "chain_id": "tacchain_2390-1",
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
* **Verification**: Confirm the chain's genesis chain\_id

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
