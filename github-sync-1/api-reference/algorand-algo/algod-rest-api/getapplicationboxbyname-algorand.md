---
description: >-
  Example code for the GetApplicationBoxByName REST method. Complete guide on
  how to use GetApplicationBoxByName REST in GetBlock Web3 documentation.
---

# GetApplicationBoxByName - Algorand

Given an application ID and box name, it returns the round, box name, and value (each base64 encoded). Box names must be in the goal app call arg encoding form `encoding:value`. For ints, use the form `int:1234`. For raw bytes, use the form `b64:A==`. For printable strings, use the form `str:hello`. For addresses, use the form `addr:XYZ...`.

## Endpoint

```http
GET /v2/applications/{application-id}/box
```

## Path Parameters

| Parameter      | Type    | Required | Description                |
| -------------- | ------- | -------- | -------------------------- |
| application-id | integer | Yes      | An application identifier. |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------- |
| name      | string | Yes      | A box name, in the goal app call arg form `encoding:value`. For ints, use the form `int:1234`. For raw bytes, use |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/applications/REPLACE_APPLICATION-ID/box"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type    | Description                                      |
| ----- | ------- | ------------------------------------------------ |
| name  | string  | The box name, base64 encoded                     |
| round | integer | The round for which this information is relevant |
| value | string  | The box value, base64 encoded.                   |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
