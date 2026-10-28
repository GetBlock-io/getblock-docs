---
description: >-
  Example code for the block JSON-RPC method. Complete guide on how to use block
  JSON-RPC in GetBlock Web3 documentation.
---

# block - NEAR Protocol

Returns detailed information about a block, selected by finality (`"final"`/`"optimistic"`) or by `block_id` (height or hash).

## Method

```
block
```

## Parameters

The `params` object carries:

| Parameter | Example   | Description |
| --------- | --------- | ----------- |
| finality  | `"final"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "block", "params": {"finality": "final"}}'
```
{% endcode %}

## Response

```json
{
    "author": "figment.poolv1.near",
    "header": {
        "height": 152000000,
        "hash": "9MzuZ...",
        "prev_hash": "8Lk...",
        "timestamp_nanosec": "1780000000000000000",
        "gas_price": "100000000"
    },
    "chunks": [
        {
            "chunk_hash": "3xY...",
            "shard_id": 0,
            "gas_used": 1200000000000
        }
    ]
}
```

## Response Fields

| Field  | Type   | Description                                                  |
| ------ | ------ | ------------------------------------------------------------ |
| author | string | The block producer's account id                              |
| header | object | Block header: height, hash, prev\_hash, timestamp, gas price |
| chunks | array  | Chunk headers, one per shard                                 |

## Use Cases

* **Block read**: Fetch a block by height, hash, or finality
* **Chunk discovery**: Read chunk\_hash values to query individual chunks

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
