---
description: >-
  Example code for the TealCompile REST method. Complete guide on how to use
  TealCompile REST in GetBlock Web3 documentation.
---

# TealCompile - Algorand

Given TEAL source code in plain text, return base64 encoded program bytes and base32 SHA512\_256 hash of program bytes (Address style). This endpoint is only enabled when a node's configuration file sets EnableDeveloperAPI to true.

## Endpoint

```http
POST /v2/teal/compile
```

## Query Parameters

| Parameter | Type    | Required | Description                                                                               |
| --------- | ------- | -------- | ----------------------------------------------------------------------------------------- |
| sourcemap | boolean | Optional | When set to `true`, returns the source map of the program as a JSON. Defaults to `false`. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/teal/compile" \
--header 'Content-Type: text/plain' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field     | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| hash      | string | base32 SHA512\_256 of program bytes (Address style) |
| result    | string | base64 encoded program bytes                        |
| sourcemap | object | JSON of the source map                              |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
