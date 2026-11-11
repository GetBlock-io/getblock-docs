---
description: >-
  Example code for the broadcast_tx_commit JSON-RPC method. Complete guide on
  how to use broadcast_tx_commit JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_commit - NEAR Protocol

Broadcasts a signed transaction and waits until it is included and executed, returning the full outcome. Equivalent to `send_tx` with `wait_until` `EXECUTED_OPTIMISTIC`.

## Method

```
broadcast_tx_commit
```

## Parameters

A positional array; the first element is shown in the example below.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_commit", "params": ["DwAAAG5lYXJrYXQudGVzdG5ldA..."]}'
```
{% endcode %}

## Response

```json
{
    "status": {
        "SuccessValue": ""
    },
    "transaction": {
        "hash": "B2d...",
        "signer_id": "example.near"
    },
    "transaction_outcome": {
        "outcome": {
            "gas_burnt": 2427979134284
        }
    },
    "receipts_outcome": []
}
```

## Response Fields

| Field                | Type   | Description               |
| -------------------- | ------ | ------------------------- |
| status               | object | Final execution status    |
| transaction          | object | The submitted transaction |
| transaction\_outcome | object | Gas burnt and status      |
| receipts\_outcome    | array  | Per-receipt outcomes      |

## Use Cases

* **Confirmed submission**: Broadcast and wait for the executed result inline

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
