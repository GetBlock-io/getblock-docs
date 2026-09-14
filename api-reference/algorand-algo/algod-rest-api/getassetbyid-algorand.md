---
description: >-
  Example code for the GetAssetByID REST method. Complete guide on how to use
  GetAssetByID REST in GetBlock Web3 documentation.
---

# GetAssetByID - Algorand

Given an asset ID, returns asset information including creator, name, total supply, and special addresses.

## Endpoint

```http
GET /v2/assets/{asset-id}
```

## Path Parameters

| Parameter | Type    | Required | Description          |
| --------- | ------- | -------- | -------------------- |
| asset-id  | integer | Yes      | An asset identifier. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/assets/REPLACE_ASSET-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type    | Description                                        |
| ------ | ------- | -------------------------------------------------- |
| index  | integer | unique asset identifier                            |
| params | object  | AssetParams specifies the parameters for an asset. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
