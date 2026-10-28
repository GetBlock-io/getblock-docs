---
description: >-
  Example code for the block_effects JSON-RPC method. Complete guide on how to
  use block_effects JSON-RPC in GetBlock Web3 documentation.
---

# block\_effects - NEAR Protocol

Returns the state changes that occurred in a block — account, access-key, data, and contract-code changes. Formerly EXPERIMENTAL\_changes\_in\_block.

## Method

```
block_effects
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
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "block_effects", "params": {"finality": "final"}}'
```
{% endcode %}

## Response

```json
{
    "block_hash": "9MzuZ...",
    "changes": [
        {
            "type": "account_touched",
            "account_id": "example.near"
        }
    ]
}
```

## Response Fields

| Field       | Type   | Description                                                             |
| ----------- | ------ | ----------------------------------------------------------------------- |
| block\_hash | string | The hash of the block the changes belong to                             |
| changes     | array  | The state-change entries in the block, each with a type and account\_id |

## Use Cases

* **Indexing**: Detect which accounts changed in a block
* **Change feeds**: Build a per-block state-change stream

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
