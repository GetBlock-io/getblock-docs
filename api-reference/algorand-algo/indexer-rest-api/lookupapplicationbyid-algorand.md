---
description: >-
  Example code for the lookupApplicationByID REST method. Complete guide on how
  to use lookupApplicationByID REST in GetBlock Web3 documentation.
---

# lookupApplicationByID - Algorand

Lookup application.

## Endpoint

```http
GET /v2/applications/{application-id}
```

## Path Parameters

| Parameter      | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| application-id | integer | Yes      | —           |

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                                            |
| ----------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| include-all | boolean | Optional | Include all items including closed accounts, deleted applications, destroyed assets, opted-out asset holdings, and closed-out application localstates. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications/REPLACE_APPLICATION-ID"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field         | Type    | Description                               |
| ------------- | ------- | ----------------------------------------- |
| application   | object  | Application index and its parameters      |
| current-round | integer | Round at which the results were computed. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
