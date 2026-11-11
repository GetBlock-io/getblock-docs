---
description: >-
  Example code for the searchForAssets REST method. Complete guide on how to use
  searchForAssets REST in GetBlock Web3 documentation.
---

# searchForAssets - Algorand

Search for assets.

## Endpoint

```http
GET /v2/assets
```

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                                            |
| ----------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| include-all | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates. |
| limit       | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                                                 |
| next        | string  | Optional | The next page of results. Use the next token provided by the previous results.                                                                         |
| creator     | string  | Optional | Filter just assets with the given creator address.                                                                                                     |
| name        | string  | Optional | Filter just assets with the given name.                                                                                                                |
| unit        | string  | Optional | Filter just assets with the given unit.                                                                                                                |
| asset-id    | integer | Optional | Asset ID                                                                                                                                               |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                                                  |
| ------------- | ------- | -------------------------------------------------------------------------------------------- |
| assets        | array   | —                                                                                            |
| current-round | integer | Round at which the results were computed.                                                    |
| next-token    | string  | Used for pagination, when making another request provide this token with the next parameter. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
