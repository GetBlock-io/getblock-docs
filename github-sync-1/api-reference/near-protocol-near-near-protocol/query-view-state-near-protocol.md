---
description: >-
  Example code for the query view state JSON-RPC method. Complete guide on how
  to use query view state JSON-RPC in GetBlock Web3 documentation.
---

# query view state - NEAR Protocol

Returns the key-value state stored under a contract account, optionally filtered by a base64 key prefix. Uses method `query` with `request_type` `view_state`.

## Method

```
query
```

## Parameters

The `params` object carries:

| Parameter       | Example          | Description |
| --------------- | ---------------- | ----------- |
| `request_type`  | `"view_state"`   | —           |
| `finality`      | `"final"`        | —           |
| `account_id`    | `"example.near"` | —           |
| `prefix_base64` | `""`             | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "query", "params": {"request_type": "view_state", "finality": "final", "account_id": "example.near", "prefix_base64": ""}}'
```
{% endcode %}

## Response

```json
{
    "values": [
        {
            "key": "U1RBVEU=",
            "value": "AQAAAA...",
            "proof": []
        }
    ],
    "block_height": 152000000,
    "block_hash": "9MzuZ..."
}
```

## Response Fields

| Field            | Type   | Description                                   |
| ---------------- | ------ | --------------------------------------------- |
| `values`         | array  | Key/value state entries (both base64-encoded) |
| `values[].key`   | string | Storage key, base64-encoded                   |
| `values[].value` | string | Storage value, base64-encoded                 |

## Use Cases

* **State reads**: Inspect a contract's raw storage
* **Indexing**: Export contract state by key prefix (use an archival node for >50kB)

## Error Handling

| Error                       | Message                  | Description                                                                                             |
| --------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| `-32700 / Parse error`      | Parse error              | The JSON request was malformed                                                                          |
| `-32601 / Method not found` | Method not found         | The method is unknown or disabled on this node version                                                  |
| `HANDLER_ERROR`             | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| `403 / RBAC: access denied` | Access denied            | The GetBlock access token is missing or incorrect                                                       |
