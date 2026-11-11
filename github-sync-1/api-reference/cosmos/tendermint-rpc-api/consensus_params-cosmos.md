---
description: >-
  Example code for the consensus_params JSON-RPC method. Complete guide on how
  to use consensus_params JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_params - Cosmos

Returns the consensus parameters (block size, evidence, and validator params) at a given height.

## Method

```
consensus_params
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
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "consensus_params", "params": {"height": "31378844"}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}consensus_params?height=31378844"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_height": "31378844",
        "consensus_params": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            },
            "evidence": {},
            "validator": {
                "pub_key_types": [
                    "ed25519"
                ]
            }
        }
    }
}
```

## Response Fields

| Field             | Type   | Description                               |
| ----------------- | ------ | ----------------------------------------- |
| block\_height     | string | Height the params apply to                |
| consensus\_params | object | Block, evidence, and validator parameters |

## Use Cases

* **Limits**: Read max block size and gas

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
