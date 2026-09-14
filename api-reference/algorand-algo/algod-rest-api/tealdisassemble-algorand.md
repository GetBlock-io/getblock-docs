---
description: >-
  Example code for the TealDisassemble REST method. Complete guide on how to use
  TealDisassemble REST in GetBlock Web3 documentation.
---

# TealDisassemble - Algorand

Given the program bytes, return the TEAL source code in plain text. This endpoint is only enabled when a node's configuration file sets EnableDeveloperAPI to true.

## Endpoint

```http
POST /v2/teal/disassemble
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/teal/disassemble" \
--header 'Content-Type: application/x-binary' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type   | Description            |
| ------ | ------ | ---------------------- |
| result | string | disassembled Teal code |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
