---
description: >-
  Example code for the GetVersion REST method. Complete guide on how to use
  GetVersion REST in GetBlock Web3 documentation.
---

# GetVersion - Algorand

Retrieves the supported API versions, binary build versions, and genesis information.

## Endpoint

```http
GET /versions
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}versions"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field              | Type   | Description |
| ------------------ | ------ | ----------- |
| build              | object | —           |
| genesis\_hash\_b64 | string | —           |
| genesis\_id        | string | —           |
| versions           | array  | —           |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
