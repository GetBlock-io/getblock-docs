---
description: >-
  Example code for the GetGenesis REST method. Complete guide on how to use
  GetGenesis REST in GetBlock Web3 documentation.
---

# GetGenesis - Algorand

Returns the entire genesis file in JSON.

## Endpoint

```http
GET /genesis
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}genesis"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field     | Type    | Description |
| --------- | ------- | ----------- |
| alloc     | array   | —           |
| comment   | string  | —           |
| devmode   | boolean | —           |
| fees      | string  | —           |
| id        | string  | —           |
| network   | string  | —           |
| proto     | string  | —           |
| rwd       | string  | —           |
| timestamp | integer | —           |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
