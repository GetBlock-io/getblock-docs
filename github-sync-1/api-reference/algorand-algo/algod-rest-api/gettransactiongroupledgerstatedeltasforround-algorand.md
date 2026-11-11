---
description: >-
  Example code for the GetTransactionGroupLedgerStateDeltasForRound REST method.
  Complete guide on how to use GetTransactionGroupLedgerStateDeltasForRound REST
  in GetBlock Web3 documentation.
---

# GetTransactionGroupLedgerStateDeltasForRound - Algorand

Get ledger deltas for transaction groups in a given round.

## Endpoint

```http
GET /v2/deltas/{round}/txn/group
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/deltas/REPLACE_ROUND/txn/group"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type  | Description |
| ------ | ----- | ----------- |
| Deltas | array | —           |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
