---
description: >-
  Example code for the GetBlock REST method. Complete guide on how to use
  GetBlock REST in GetBlock Web3 documentation.
---

# GetBlock - Algorand

Get the block for the given round.

## Endpoint

```http
GET /v2/blocks/{round}
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                               |
| ----------- | ------- | -------- | --------------------------------------------------------------------------------------------------------- |
| header-only | boolean | Optional | If true, only the block header (exclusive of payset or certificate) may be included in response.          |
| format      | string  | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type   | Description                                                                                |
| ----- | ------ | ------------------------------------------------------------------------------------------ |
| block | object | Block header data.                                                                         |
| cert  | object | Optional certificate object. This is only included when the format is set to message pack. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
