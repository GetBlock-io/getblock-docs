---
description: >-
  Example code for the lookupAssetByID REST method. Complete guide on how to use
  lookupAssetByID REST in GetBlock Web3 documentation.
---

# lookupAssetByID - Algorand

Lookup asset information.

## Endpoint

```http
GET /v2/assets/{asset-id}
```

## Path Parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| asset-id  | integer | Yes      | —           |

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                                            |
| ----------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| include-all | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/assets/REPLACE_ASSET-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                                          |
| ------------- | ------- | -------------------------------------------------------------------- |
| asset         | object  | Specifies both the unique identifier and the parameters for an asset |
| current-round | integer | Round at which the results were computed.                            |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
