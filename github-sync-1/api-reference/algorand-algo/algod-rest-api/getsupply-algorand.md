---
description: >-
  Example code for the GetSupply REST method. Complete guide on how to use
  GetSupply REST in GetBlock Web3 documentation.
---

# GetSupply - Algorand

Get the current supply reported by the ledger.

## Endpoint

```http
GET /v2/ledger/supply
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/ledger/supply"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field          | Type    | Description                                                                                                                                                    |
| -------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| current\_round | integer | Round                                                                                                                                                          |
| online-money   | integer | Total stake held by accounts with status Online at current\_round, including those whose participation keys have expired but have not yet been marked offline. |
| online-stake   | integer | Online stake used by agreement to vote for current\_round, excluding accounts whose participation keys have expired.                                           |
| total-money    | integer | TotalMoney                                                                                                                                                     |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
