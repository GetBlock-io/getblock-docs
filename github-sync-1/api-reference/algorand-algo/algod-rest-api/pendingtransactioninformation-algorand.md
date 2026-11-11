---
description: >-
  Example code for the PendingTransactionInformation REST method. Complete guide
  on how to use PendingTransactionInformation REST in GetBlock Web3
  documentation.
---

# PendingTransactionInformation - Algorand

Given a transaction ID of a recently submitted transaction, it returns information about it. There are several cases when this might succeed:

* Transaction committed (committed round > 0)
* Transaction still in the pool (committed round = 0, pool error = "")
* Transaction removed from pool due to error (committed round = 0, pool error != "")

Or the transaction may have happened sufficiently long ago that the node no longer remembers it, and this will return an error.

## Endpoint

```http
GET /v2/transactions/pending/{txid}
```

## Path Parameters

| Parameter | Type   | Required | Description      |
| --------- | ------ | -------- | ---------------- |
| txid      | string | Yes      | A transaction ID |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/transactions/pending/REPLACE_TXID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                | Type    | Description                                                                                                                                                     |
| -------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| application-index    | integer | The application index if the transaction was found and it created an application.                                                                               |
| asset-closing-amount | integer | The number of the asset's unit that were transferred to the close-to address.                                                                                   |
| asset-index          | integer | The asset index if the transaction was found and it created an asset.                                                                                           |
| close-rewards        | integer | Rewards in microalgos applied to the close remainder to account.                                                                                                |
| closing-amount       | integer | Closing amount for the transaction.                                                                                                                             |
| confirmed-round      | integer | The round where this transaction was confirmed, if present.                                                                                                     |
| global-state-delta   | array   | Application state delta.                                                                                                                                        |
| inner-txns           | array   | Inner transactions produced by application execution.                                                                                                           |
| local-state-delta    | array   | Local state key/value changes for the application being executed by this transaction.                                                                           |
| logs                 | array   | Logs for the application being executed by this transaction.                                                                                                    |
| pool-error           | string  | Indicates that the transaction was kicked out of this node's transaction pool (and specifies why that happened). An empty string indicates the transaction wasn |
| receiver-rewards     | integer | Rewards in microalgos applied to the receiver account.                                                                                                          |
| sender-rewards       | integer | Rewards in microalgos applied to the sender account.                                                                                                            |
| txn                  | object  | The raw signed transaction.                                                                                                                                     |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
