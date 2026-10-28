---
description: >-
  Example code for the send_tx JSON-RPC method. Complete guide on how to use
  send_tx JSON-RPC in GetBlock Web3 documentation.
---

# send\_tx - NEAR Protocol

Broadcasts a signed transaction and waits for the requested execution milestone (`wait_until`: `NONE`, `INCLUDED`, `EXECUTED_OPTIMISTIC`, `FINAL`, etc.). Recommended for new integrations.

## Method

```
send_tx
```

## Parameters

The `params` object carries:

| Parameter          | Example                           | Description |
| ------------------ | --------------------------------- | ----------- |
| `signed_tx_base64` | `"DwAAAG5lYXJrYXQudGVzdG5ldA..."` | —           |
| `wait_until`       | `"EXECUTED_OPTIMISTIC"`           | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "send_tx", "params": {"signed_tx_base64": "DwAAAG5lYXJrYXQudGVzdG5ldA...", "wait_until": "EXECUTED_OPTIMISTIC"}}'
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
        "signer_id": "example.near",
        "receiver_id": "token.near"
    },
    "transaction_outcome": {
        "id": "B2d...",
        "outcome": {
            "gas_burnt": 2427979134284,
            "status": {
                "SuccessValue": ""
            }
        }
    },
    "receipts_outcome": []
}
```

## Response Fields

| Field                 | Type   | Description                                                         |
| --------------------- | ------ | ------------------------------------------------------------------- |
| `status`              | object | Final execution status (SuccessValue, SuccessReceiptId, or Failure) |
| `transaction`         | object | The submitted transaction                                           |
| `transaction_outcome` | object | Gas burnt and status of the transaction's conversion to receipts    |
| `receipts_outcome`    | array  | Per-receipt execution outcomes                                      |

## Use Cases

* **Submission**: Broadcast a signed transaction and choose how long to wait
* **Confirmation**: Use `wait_until` `EXECUTED_OPTIMISTIC` or `FINAL` for a final result

## Error Handling

| Error                       | Message                  | Description                                                                                             |
| --------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| `-32700 / Parse error`      | Parse error              | The JSON request was malformed                                                                          |
| `-32601 / Method not found` | Method not found         | The method is unknown or disabled on this node version                                                  |
| `HANDLER_ERROR`             | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| `403 / RBAC: access denied` | Access denied            | The GetBlock access token is missing or incorrect                                                       |
