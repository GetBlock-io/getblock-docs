---
description: >-
  Example code for the RawTransactionAsync REST method. Complete guide on how to
  use RawTransactionAsync REST in GetBlock Web3 documentation.
---

# RawTransactionAsync - Algorand

Fast track for broadcasting a raw transaction or transaction group to the network through the tx handler without performing most of the checks and reporting detailed errors. Should be only used for development and performance testing.

## Endpoint

```http
POST /v2/transactions/async
```

## Query Parameters

| Parameter             | Type    | Required | Description                                                                                                                                                     |
| --------------------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| skip-pq-address-check | boolean | Optional | Skip post-quantum address checks, including the check that rejects PQ authorizer and LogicSig escrow (TEAL v13 or later) whose address is an Edwards25519 curve |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/transactions/async" \
--header 'Content-Type: application/x-binary' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description   |
| ------ | ---------------- | ------------- |
| (body) | application/json | Response body |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
