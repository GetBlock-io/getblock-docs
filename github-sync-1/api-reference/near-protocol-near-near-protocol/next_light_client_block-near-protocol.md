---
description: >-
  Example code for the next_light_client_block JSON-RPC method. Complete guide
  on how to use next_light_client_block JSON-RPC in GetBlock Web3 documentation.
---

# next\_light\_client\_block - NEAR Protocol

Returns the next light-client block after the given known block hash — the data a light client needs to advance and verify the chain without a full node.

## Method

```
next_light_client_block
```

## Parameters

The `params` object carries:

| Parameter         | Example                   | Description |
| ----------------- | ------------------------- | ----------- |
| last\_block\_hash | `"9MzuZ...lastknownhash"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "next_light_client_block", "params": {"last_block_hash": "9MzuZ...lastknownhash"}}'
```
{% endcode %}

## Response

```json
{
    "prev_block_hash": "9MzuZ...",
    "next_block_inner_hash": "7Ab...",
    "inner_lite": {
        "height": 152000050,
        "epoch_id": "8Lk...",
        "timestamp": "1780000000000000000"
    },
    "next_bps": [],
    "approvals_after_next": []
}
```

## Response Fields

| Field                  | Type   | Description                                                        |
| ---------------------- | ------ | ------------------------------------------------------------------ |
| inner\_lite            | object | The compact header (height, epoch, timestamp, next-validator hash) |
| next\_bps              | array  | The next block producers, when the epoch changes                   |
| approvals\_after\_next | array  | Validator approvals used to verify the block                       |

## Use Cases

* **Light clients**: Advance a light client to the next verifiable block
* **Bridges**: Supply headers to a trustless NEAR light-client bridge

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
