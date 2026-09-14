---
description: >-
  Example code for the GetLedgerStateDeltaForTransactionGroup REST method.
  Complete guide on how to use GetLedgerStateDeltaForTransactionGroup REST in
  GetBlock Web3 documentation.
---

# GetLedgerStateDeltaForTransactionGroup - Algorand

Get a ledger delta for a given transaction group.

## Endpoint

```http
GET /v2/deltas/txn/group/{id}
```

## Path Parameters

| Parameter | Type   | Required | Description                               |
| --------- | ------ | -------- | ----------------------------------------- |
| id        | string | Yes      | A transaction ID, or transaction group ID |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/deltas/txn/group/REPLACE_ID"
```
{% endcode %}

## Response

Returns HTTP 200 on success.

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
