---
description: >-
  Example code for the GetPendingTransactionsByAddress REST method. Complete
  guide on how to use GetPendingTransactionsByAddress REST in GetBlock Web3
  documentation.
---

# GetPendingTransactionsByAddress - Algorand

Get the list of pending transactions by address, sorted by priority in decreasing order and truncated at the end at MAX. If MAX = 0, returns all pending transactions.

## Endpoint

```http
GET /v2/accounts/{address}/transactions/pending
```

## Path Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| address   | string | Yes      | An account public key. |

## Query Parameters

| Parameter | Type    | Required | Description                                                                                               |
| --------- | ------- | -------- | --------------------------------------------------------------------------------------------------------- |
| max       | integer | Optional | Truncated number of transactions to display. If max=0, returns all pending txns.                          |
| format    | string  | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/REPLACE_ADDRESS/transactions/pending"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field              | Type    | Description                               |
| ------------------ | ------- | ----------------------------------------- |
| top-transactions   | array   | An array of signed transaction objects.   |
| total-transactions | integer | Total number of transactions in the pool. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
