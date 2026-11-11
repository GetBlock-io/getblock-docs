---
description: >-
  Example code for the changes JSON-RPC method. Complete guide on how to use
  changes JSON-RPC in GetBlock Web3 documentation.
---

# changes - NEAR Protocol

Returns fine-grained state changes filtered by a `changes_type`. Supported `changes_type` values include `account_changes`, `single_access_key_changes`, `all_access_key_changes`, `data_changes`, and `contract_code_changes`, each with its own required params.

## Method

```
changes
```

## Parameters

The `params` object carries:

| Parameter      | Example             | Description |
| -------------- | ------------------- | ----------- |
| `changes_type` | `"account_changes"` | —           |
| `account_ids`  | `["example.near"]`  | —           |
| `finality`     | `"final"`           | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "changes", "params": {"changes_type": "account_changes", "account_ids": ["example.near"], "finality": "final"}}'
```
{% endcode %}

## Response

```json
{
    "block_hash": "9MzuZ...",
    "changes": [
        {
            "cause": {
                "type": "transaction_processing",
                "tx_hash": "B2d..."
            },
            "type": "account_update",
            "change": {
                "account_id": "example.near",
                "amount": "125000000000000000000000000"
            }
        }
    ]
}
```

## Response Fields

| Field             | Type   | Description                                             |
| ----------------- | ------ | ------------------------------------------------------- |
| `block_hash`      | string | The block the changes belong to                         |
| `changes`         | array  | Change entries, each with a cause and the updated value |
| `changes[].cause` | object | What triggered the change (transaction, receipt, etc.)  |

## Use Cases

* **Account indexing**: Track balance and state changes for specific accounts
* **Audit trails**: Attribute each change to its causing transaction or receipt

## Error Handling

| Error                       | Message                  | Description                                                                                             |
| --------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| `-32700 / Parse error`      | Parse error              | The JSON request was malformed                                                                          |
| `-32601 / Method not found` | Method not found         | The method is unknown or disabled on this node version                                                  |
| `HANDLER_ERROR`             | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| `403 / RBAC: access denied` | Access denied            | The GetBlock access token is missing or incorrect                                                       |
