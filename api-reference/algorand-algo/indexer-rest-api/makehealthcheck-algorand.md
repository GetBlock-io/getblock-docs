---
description: >-
  Example code for the makeHealthCheck REST method. Complete guide on how to use
  makeHealthCheck REST in GetBlock Web3 documentation.
---

# makeHealthCheck - Algorand

Returns 200 if healthy.

## Endpoint

```http
GET /health
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}health"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field        | Type    | Description      |
| ------------ | ------- | ---------------- |
| data         | object  | —                |
| db-available | boolean | —                |
| errors       | array   | —                |
| is-migrating | boolean | —                |
| message      | string  | —                |
| round        | integer | —                |
| version      | string  | Current version. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
