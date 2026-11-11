---
description: >-
  Example code for the lookupApplicationBoxByIDAndName REST method. Complete
  guide on how to use lookupApplicationBoxByIDAndName REST in GetBlock Web3
  documentation.
---

# lookupApplicationBoxByIDAndName - Algorand

Given an application ID and box name, returns base64 encoded box name and value. Box names must be in the goal app call arg form `encoding:value`. For ints, use the form `int:1234`. For raw bytes, encode base 64 and use `b64` prefix as in `b64:A==`. For printable strings, use the form `str:hello`. For addresses, use the form `addr:XYZ...`.

## Endpoint

```http
GET /v2/applications/{application-id}/box
```

## Path Parameters

| Parameter      | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| application-id | integer | Yes      | —           |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                                                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | string | Yes      | A box name in goal-arg form `encoding:value`. For ints, use the form `int:1234`. For raw bytes, use the form `b64:A==`. For printable strings, use the form `str` |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications/REPLACE_APPLICATION-ID/box"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type    | Description                                      |
| ----- | ------- | ------------------------------------------------ |
| name  | string  | \[name] box name, base64 encoded                 |
| round | integer | The round for which this information is relevant |
| value | string  | \[value] box value, base64 encoded.              |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
