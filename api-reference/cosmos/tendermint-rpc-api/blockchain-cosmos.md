---
description: >-
  Example code for the blockchain JSON-RPC method. Complete guide on how to use
  blockchain JSON-RPC in GetBlock Web3 documentation.
---

# blockchain - Cosmos

Returns block metadata (headers) for a range of heights, newest first. The range is capped at 20 blocks per call.

## Method

```
blockchain
```

## Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| minHeight | string | Lowest block height in the range  |
| maxHeight | string | Highest block height in the range |

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${COSMOS_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "blockchain", "params": {"minHeight": "31378800", "maxHeight": "31378820"}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}blockchain?minHeight=31378800&maxHeight=31378820"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "last_height": "31378844",
        "block_metas": [
            {
                "block_id": {
                    "hash": "A1B2..."
                },
                "header": {
                    "height": "31378820",
                    "time": "2026-06-01T10:31:22Z"
                },
                "num_txs": "3"
            }
        ]
    }
}
```

## Response Fields

| Field        | Type   | Description                                        |
| ------------ | ------ | -------------------------------------------------- |
| last\_height | string | Current chain tip height                           |
| block\_metas | array  | Block headers in the requested range, newest first |

## Use Cases

* **Range scan**: Pull a window of recent block headers

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
