---
description: >-
  Example code for the GetReady REST method. Complete guide on how to use
  GetReady REST in GetBlock Web3 documentation.
---

# GetReady - Algorand

Returns OK if healthy and fully caught up.

## Endpoint

```http
GET /ready
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}ready"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description |
| ------ | ---------------- | ----------- |
| (body) | application/json | OK.         |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
