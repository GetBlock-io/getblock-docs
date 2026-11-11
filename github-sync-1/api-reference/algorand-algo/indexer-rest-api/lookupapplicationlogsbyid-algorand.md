---
description: >-
  Example code for the lookupApplicationLogsByID REST method. Complete guide on
  how to use lookupApplicationLogsByID REST in GetBlock Web3 documentation.
---

# lookupApplicationLogsByID - Algorand

Lookup application logs.

## Endpoint

```http
GET /v2/applications/{application-id}/logs
```

## Path Parameters

| Parameter      | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| application-id | integer | Yes      | —           |

## Query Parameters

| Parameter      | Type    | Required | Description                                                                                            |
| -------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------ |
| limit          | integer | Optional | Maximum number of results to return. There could be additional pages even if the limit is not reached. |
| next           | string  | Optional | The next page of results. Use the next token provided by the previous results.                         |
| txid           | string  | Optional | Lookup the specific transaction by ID.                                                                 |
| min-round      | integer | Optional | Include results at or after the specified min-round.                                                   |
| max-round      | integer | Optional | Include results at or before the specified max-round.                                                  |
| sender-address | string  | Optional | Only include transactions with this sender address.                                                    |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/applications/REPLACE_APPLICATION-ID/logs"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field          | Type    | Description                                                                                  |
| -------------- | ------- | -------------------------------------------------------------------------------------------- |
| application-id | integer | \[appidx] application index.                                                                 |
| current-round  | integer | Round at which the results were computed.                                                    |
| log-data       | array   | —                                                                                            |
| next-token     | string  | Used for pagination, when making another request provide this token with the next parameter. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
