---
description: >-
  Example code for the tx JSON-RPC method. Complete guide on how to use tx
  JSON-RPC in GetBlock Web3 documentation.
---

# tx - NEAR Protocol

Returns the status of a previously submitted transaction, by `tx_hash` plus `sender_account_id` (or by `signed_tx_base64`). Recommended for new integrations.

## Method

```
tx
```

## Parameters

The `params` object carries:

| Parameter           | Example                                          | Description |
| ------------------- | ------------------------------------------------ | ----------- |
| tx\_hash            | `"B2dEThkFLWv8y5xjNUSW96NU7mKGyRk8sLh1G8sB8qL9"` | —           |
| sender\_account\_id | `"example.near"`                                 | —           |
| wait\_until         | `"EXECUTED_OPTIMISTIC"`                          | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "tx", "params": {"tx_hash": "B2dEThkFLWv8y5xjNUSW96NU7mKGyRk8sLh1G8sB8qL9", "sender_account_id": "example.near", "wait_until": "EXECUTED_OPTIMISTIC"}}'
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

| Field                | Type   | Description                         |
| -------------------- | ------ | ----------------------------------- |
| status               | object | Execution status of the transaction |
| transaction          | object | The transaction's fields            |
| transaction\_outcome | object | Gas burnt and status                |
| receipts\_outcome    | array  | Per-receipt execution outcomes      |

## Use Cases

* **Receipt lookup**: Resolve a transaction's outcome by hash
* **Polling**: Follow an async submission to completion

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
