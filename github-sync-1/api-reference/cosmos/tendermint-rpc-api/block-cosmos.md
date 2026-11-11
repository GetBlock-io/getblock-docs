---
description: >-
  Example code for the block JSON-RPC method. Complete guide on how to use block
  JSON-RPC in GetBlock Web3 documentation.
---

# block - Cosmos

Returns the block at a given height, including its header, data (transactions), and commit. Omitting height returns the latest block.

## Method

```
block
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
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "block", "params": {"height": "31378844"}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}block?height=31378844"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_id": {
            "hash": "A1B2..."
        },
        "block": {
            "header": {
                "height": "31378844",
                "time": "2026-06-01T10:31:22Z",
                "chain_id": "cosmoshub-4"
            },
            "data": {
                "txs": [
                    "Cg8K..."
                ]
            }
        }
    }
}
```

## Response Fields

| Field     | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| block\_id | object | The block's id and hash                                  |
| block     | object | The block header, data (txs), evidence, and last\_commit |

## Use Cases

* **Block read**: Fetch a full block by height
* **Tx extraction**: Read base64-encoded txs from block.data.txs

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
