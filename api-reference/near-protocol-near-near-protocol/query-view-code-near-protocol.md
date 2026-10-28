---
description: >-
  Example code for the query view code JSON-RPC method. Complete guide on how to
  use query view code JSON-RPC in GetBlock Web3 documentation.
---

# query view code - NEAR Protocol

Returns the deployed contract code (WASM, base64-encoded) for an account, with its hash. Uses method `query` with `request_type` `view_code`.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter     | Example          | Description |
| ------------- | ---------------- | ----------- |
| request\_type | `"view_code"`    | —           |
| finality      | `"final"`        | —           |
| account\_id   | `"example.near"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "view_code", "finality": "final", "account_id": "example.near"}}'
```
{% endcode %}

## Response

```json
{
    "code_base64": "AGFzbQEAAAA...",
    "hash": "7KoFshMz...",
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field         | Type   | Description                                |
| ------------- | ------ | ------------------------------------------ |
| code\_base64  | string | The contract WASM bytecode, base64-encoded |
| hash          | string | Hash of the contract code                  |
| block\_height | number | Height the state was read at               |

## Use Cases

* **Verification**: Fetch and hash the deployed WASM of a contract
* **Audit**: Compare on-chain code against a known build

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
