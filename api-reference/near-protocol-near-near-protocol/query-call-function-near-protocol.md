---
description: >-
  Example code for the query call function JSON-RPC method. Complete guide on
  how to use query call function JSON-RPC in GetBlock Web3 documentation.
---

# query call function - NEAR Protocol

Executes a read-only contract view method and returns its result, without creating a transaction. Uses method query with request\_type call\_function; args are base64-encoded JSON.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter     | Example                                      | Description |
| ------------- | -------------------------------------------- | ----------- |
| request\_type | `"call_function"`                            | —           |
| finality      | `"final"`                                    | —           |
| account\_id   | `"example.near"`                             | —           |
| method\_name  | `"ft_balance_of"`                            | —           |
| args\_base64  | `"eyJhY2NvdW50X2lkIjoiZXhhbXBsZS5uZWFyIn0="` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "call_function", "finality": "final", "account_id": "example.near", "method_name": "ft_balance_of", "args_base64": "eyJhY2NvdW50X2lkIjoiZXhhbXBsZS5uZWFyIn0="}}'
```
{% endcode %}

## Response

```json
{
    "result": [
        49,
        50,
        53,
        48,
        48,
        48
    ],
    "logs": [],
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field         | Type   | Description                                                      |
| ------------- | ------ | ---------------------------------------------------------------- |
| result        | array  | The method's return value as a byte array (decode to UTF-8/JSON) |
| logs          | array  | Logs emitted during the view call                                |
| block\_height | number | Height the call was executed at                                  |

## Use Cases

* **Contract reads**: Call ft\_balance\_of, nft\_tokens, or any view method
* **dApp UI**: Populate views without submitting a transaction

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
