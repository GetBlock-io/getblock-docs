---
description: >-
  Example code for the chunk JSON-RPC method. Complete guide on how to use chunk
  JSON-RPC in GetBlock Web3 documentation.
---

# chunk - NEAR Protocol

Returns the details of a chunk, by chunk hash, or by block\_id plus shard\_id. Includes the chunk header and its transactions and receipts.

## Method

```
chunk
```

## Parameters

The `params` object carries:

| Parameter | Example               | Description |
| --------- | --------------------- | ----------- |
| chunk\_id | `"3xY7k...chunkhash"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "chunk", "params": {"chunk_id": "3xY7k...chunkhash"}}'
```
{% endcode %}

## Response

```json
{
    "header": {
        "chunk_hash": "3xY...",
        "shard_id": 0,
        "height_created": 152000000,
        "gas_used": 1200000000000,
        "gas_limit": 1000000000000000
    },
    "transactions": [
        {
            "hash": "B2d...",
            "signer_id": "example.near",
            "receiver_id": "token.near"
        }
    ],
    "receipts": []
}
```

## Response Fields

| Field        | Type   | Description                                           |
| ------------ | ------ | ----------------------------------------------------- |
| header       | object | Chunk header: hash, shard\_id, height, gas used/limit |
| transactions | array  | Transactions included in the chunk                    |
| receipts     | array  | Receipts processed in the chunk                       |

## Use Cases

* **Transaction extraction**: Read a shard's transactions from a chunk
* **Shard data**: Inspect per-shard gas usage

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
