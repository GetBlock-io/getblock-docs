---
description: >-
  Example code for the GetBlockLogs REST method. Complete guide on how to use
  GetBlockLogs REST in GetBlock Web3 documentation.
---

# GetBlockLogs - Algorand

Get all of the logs from outer and inner app calls in the given round.

## Endpoint

```http
GET /v2/blocks/{round}/logs
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND/logs"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type  | Description |
| ----- | ----- | ----------- |
| logs  | array | —           |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
