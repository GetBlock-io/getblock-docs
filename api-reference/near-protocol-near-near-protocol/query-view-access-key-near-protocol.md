---
description: >-
  Example code for the query view access key JSON-RPC method. Complete guide on
  how to use query view access key JSON-RPC in GetBlock Web3 documentation.
---

# query view access key - NEAR Protocol

Returns information about a single access key on an account: its nonce and permission (full access or a function-call permission). Uses method `query` with `request_type` `view_access_key`.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter     | Example                                                  | Description |
| ------------- | -------------------------------------------------------- | ----------- |
| request\_type | `"view_access_key"`                                      | —           |
| finality      | `"final"`                                                | —           |
| account\_id   | `"example.near"`                                         | —           |
| public\_key   | `"ed25519:DcA2MzgpJbrUATQLLceocVckhhAqrkingax4oJ9kZ847"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "view_access_key", "finality": "final", "account_id": "example.near", "public_key": "ed25519:DcA2MzgpJbrUATQLLceocVckhhAqrkingax4oJ9kZ847"}}'
```
{% endcode %}

## Response

```json
{
    "nonce": 85,
    "permission": {
        "FunctionCall": {
            "allowance": "250000000000000000000000",
            "receiver_id": "token.near",
            "method_names": []
        }
    },
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field         | Type   | Description                                                          |
| ------------- | ------ | -------------------------------------------------------------------- |
| nonce         | number | The key's current nonce (increment for the next transaction)         |
| permission    | object | FullAccess, or a FunctionCall permission with allowance and receiver |
| block\_height | number | Height the state was read at                                         |

## Use Cases

* **Nonce management**: Read the key nonce before signing a transaction
* **Permission check**: Confirm a function-call key's allowance and receiver

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
