---
description: >-
  Example code for the tx_status JSON-RPC method. Complete guide on how to use
  tx_status JSON-RPC in GetBlock Web3 documentation.
---

# tx\_status - NEAR Protocol

Returns the transaction status plus the full list of receipts. Available from nearcore 2.14.0; on older nodes use EXPERIMENTAL\_tx\_status (identical request and response).

## Method

```
tx_status
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
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "tx_status", "params": {"tx_hash": "B2dEThkFLWv8y5xjNUSW96NU7mKGyRk8sLh1G8sB8qL9", "sender_account_id": "example.near", "wait_until": "EXECUTED_OPTIMISTIC"}}'
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
    "receipts_outcome": [],
    "receipts": []
}
```

## Response Fields

| Field                | Type   | Description                                   |
| -------------------- | ------ | --------------------------------------------- |
| status               | object | Execution status of the transaction           |
| transaction\_outcome | object | Gas burnt and status                          |
| receipts             | array  | The full receipts produced by the transaction |
| receipts\_outcome    | array  | Per-receipt execution outcomes                |

## Use Cases

* **Full trace**: Read a transaction with its complete receipt tree
* **Cross-contract debugging**: Follow receipts across contracts

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
