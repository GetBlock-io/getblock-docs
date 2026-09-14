---
description: >-
  Example code for the ExperimentalCheck REST method. Complete guide on how to
  use ExperimentalCheck REST in GetBlock Web3 documentation.
---

# ExperimentalCheck - Algorand

Returns OK if experimental API is enabled.

## Endpoint

```http
GET /v2/experimental
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/experimental"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description              |
| ------ | ---------------- | ------------------------ |
| (body) | application/json | Experimental API enabled |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
