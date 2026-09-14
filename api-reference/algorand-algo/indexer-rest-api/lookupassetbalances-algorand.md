---
description: >-
  Example code for the lookupAssetBalances REST method. Complete guide on how to
  use lookupAssetBalances REST in GetBlock Web3 documentation.
---

# lookupAssetBalances - Algorand

Lookup the list of accounts who hold this asset.

## Endpoint

```http
GET /v2/assets/{asset-id}/balances
```

## Path Parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| asset-id  | integer | Yes      | —           |

## Query Parameters

| Parameter             | Type    | Required | Description                                                                                                                                                      |
| --------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| include-all           | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates.           |
| limit                 | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                                                           |
| next                  | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                                                   |
| currency-greater-than | integer | Optional | Results should have an amount greater than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used. |
| currency-less-than    | integer | Optional | Results should have an amount less than this value. MicroAlgos are the default currency unless an asset-id is provided, in which case the asset will be used.    |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets/REPLACE_ASSET-ID/balances"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                  |
| ------------- | ------- | -------------------------------------------------------------------------------------------- |
| balances      | array   | —                                                                                            |
| current-round | integer | Round at which the results were computed.                                                    |
| next-token    | string  | Used for pagination, when making another request provide this token with the next parameter. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
