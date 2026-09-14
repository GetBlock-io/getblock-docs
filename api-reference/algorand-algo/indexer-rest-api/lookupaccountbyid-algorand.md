---
description: >-
  Example code for the lookupAccountByID REST method. Complete guide on how to
  use lookupAccountByID REST in GetBlock Web3 documentation.
---

# lookupAccountByID - Algorand

Lookup account information.

## Endpoint

```http
GET /v2/accounts/{account-id}
```

## Path Parameters

| Parameter  | Type   | Required | Description    |
| ---------- | ------ | -------- | -------------- |
| account-id | string | Yes      | account string |

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                                                      |
| ----------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| round       | integer | Optional | Include results for the specified round.                                                                                                                         |
| include-all | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates.           |
| exclude     | array   | Optional | Exclude additional items such as asset holdings, application local data stored for this account, asset parameters created by this account, and application param |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/accounts/REPLACE_ACCOUNT-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                               |
| ------------- | ------- | ----------------------------------------- |
| account       | object  | Account information at a given round.     |
| current-round | integer | Round at which the results were computed. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
