---
description: >-
  Example code for the query view account JSON-RPC method. Complete guide on how
  to use query view account JSON-RPC in GetBlock Web3 documentation.
---

# query view account - NEAR Protocol

Returns the basic account information for a NEAR account: balance, locked stake, storage usage, and the hash of its contract code (if any). Uses method `query` with `request_type` `view_account`.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter     | Example          | Description |
| ------------- | ---------------- | ----------- |
| request\_type | `"view_account"` | —           |
| finality      | `"final"`        | —           |
| account\_id   | `"example.near"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "view_account", "finality": "final", "account_id": "example.near"}}'
```
{% endcode %}

## Response

```json
{
    "amount": "125000000000000000000000000",
    "locked": "0",
    "code_hash": "11111111111111111111111111111111",
    "storage_usage": 2080,
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field          | Type   | Description                                            |
| -------------- | ------ | ------------------------------------------------------ |
| amount         | string | Liquid balance in yoctoNEAR (1 NEAR = 10^24 yoctoNEAR) |
| locked         | string | Staked (locked) balance in yoctoNEAR                   |
| code\_hash     | string | Hash of the deployed contract code (all-ones if none)  |
| storage\_usage | number | Bytes of storage the account uses                      |

## Use Cases

* **Balance display**: Read an account's NEAR balance (divide amount by 1e24)
* **Contract check**: A non-default code\_hash means a contract is deployed

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
