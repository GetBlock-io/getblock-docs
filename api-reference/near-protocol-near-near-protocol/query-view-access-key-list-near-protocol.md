---
description: >-
  Example code for the query view access key list JSON-RPC method. Complete
  guide on how to use query view access key list JSON-RPC in GetBlock Web3
  documentation.
---

# query view access key list - NEAR Protocol

Returns all access keys registered to an account, each with its public key, nonce, and permission. Uses method `query` with `request_type` `view_access_key_list`.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter     | Example                  | Description |
| ------------- | ------------------------ | ----------- |
| request\_type | `"view_access_key_list"` | —           |
| finality      | `"final"`                | —           |
| account\_id   | `"example.near"`         | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "view_access_key_list", "finality": "final", "account_id": "example.near"}}'
```
{% endcode %}

## Response

```json
{
    "keys": [
        {
            "public_key": "ed25519:DcA2MzgpJbrUATQLLceocVckhhAqrkingax4oJ9kZ847",
            "access_key": {
                "nonce": 85,
                "permission": "FullAccess"
            }
        }
    ],
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field               | Type   | Description                    |
| ------------------- | ------ | ------------------------------ |
| keys                | array  | The account's access keys      |
| keys\[].public\_key | string | The key's public key           |
| keys\[].access\_key | object | The key's nonce and permission |

## Use Cases

* **Key management**: List an account's keys in a wallet UI
* **Security audit**: Review which keys can act for an account

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
