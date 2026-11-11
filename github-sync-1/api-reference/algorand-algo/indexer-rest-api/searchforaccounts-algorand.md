---
description: >-
  Example code for the searchForAccounts REST method. Complete guide on how to
  use searchForAccounts REST in GetBlock Web3 documentation.
---

# searchForAccounts - Algorand

Search for accounts.

## Endpoint

```http
GET /v2/accounts
```

## Query Parameters

| Parameter             | Type    | Required | Description                                                                                                                                                      |
| --------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| asset-id              | integer | Optional | Asset ID                                                                                                                                                         |
| limit                 | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                                                           |
| next                  | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                                                   |
| currency-greater-than | integer | Optional | Results should have an amount greater than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used. |
| include-all           | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates.           |
| exclude               | array   | Optional | Exclude additional items such as asset holdings, application local data stored for this account, asset parameters created by this account, and application param |
| currency-less-than    | integer | Optional | Results should have an amount less than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used.    |
| auth-addr             | string  | Optional | Include accounts configured to use this spending key.                                                                                                            |
| round                 | integer | Optional | Include results for the specified round. For performance reasons, this parameter may be disabled on some configurations. Using application-id or asset-id filter |
| application-id        | integer | Optional | Application ID                                                                                                                                                   |
| online-only           | boolean | Optional | When this is set to true, return only accounts whose participation status is currently online.                                                                   |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/accounts"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                  |
| ------------- | ------- | -------------------------------------------------------------------------------------------- |
| accounts      | array   | —                                                                                            |
| current-round | integer | Round at which the results were computed.                                                    |
| next-token    | string  | Used for pagination, when making another request provide this token with the next parameter. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
