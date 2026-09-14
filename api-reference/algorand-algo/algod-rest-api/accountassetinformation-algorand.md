---
description: >-
  Example code for the AccountAssetInformation REST method. Complete guide on
  how to use AccountAssetInformation REST in GetBlock Web3 documentation.
---

# AccountAssetInformation - Algorand

Given a specific account public key and asset ID, this call returns the account's asset holding and asset parameters (if either exist). Asset parameters will only be returned if the provided address is the asset's creator.

## Endpoint

```http
GET /v2/accounts/{address}/assets/{asset-id}
```

## Path Parameters

| Parameter | Type    | Required | Description            |
| --------- | ------- | -------- | ---------------------- |
| address   | string  | Yes      | An account public key. |
| asset-id  | integer | Yes      | An asset identifier.   |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/accounts/REPLACE_ADDRESS/assets/REPLACE_ASSET-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                                        |
| ------------- | ------- | -------------------------------------------------- |
| asset-holding | object  | Describes an asset held by an account.             |
| created-asset | object  | AssetParams specifies the parameters for an asset. |
| round         | integer | The round for which this information is relevant.  |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
