---
description: >-
  Example code for the searchForApplicationBoxes REST method. Complete guide on
  how to use searchForApplicationBoxes REST in GetBlock Web3 documentation.
---

# searchForApplicationBoxes - Algorand

Given an application ID, returns the box names of that application sorted lexicographically.

## Endpoint

```http
GET /v2/applications/{application-id}/boxes
```

## Path Parameters

| Parameter      | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| application-id | integer | Yes      | —           |

## Query Parameters

| Parameter | Type    | Required | Description                                                                                                           |
| --------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------- |
| limit     | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached.                |
| next      | string  | Optional | The next page of results. Use the next token provided by the previous results.                                        |
| include   | array   | Optional | Include additional items in the response. Use `values` to include box values. Multiple values can be comma-separated. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications/REPLACE_APPLICATION-ID/boxes"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field          | Type    | Description                                                                                  |
| -------------- | ------- | -------------------------------------------------------------------------------------------- |
| application-id | integer | \[appidx] application index.                                                                 |
| boxes          | array   | —                                                                                            |
| next-token     | string  | Used for pagination, when making another request provide this token with the next parameter. |
| round          | integer | The round for which this information is relevant.                                            |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
