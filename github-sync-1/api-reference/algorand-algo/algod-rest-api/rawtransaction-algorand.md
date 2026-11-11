---
description: >-
  Example code for the RawTransaction REST method. Complete guide on how to use
  RawTransaction REST in GetBlock Web3 documentation.
---

# RawTransaction - Algorand

Broadcasts a raw transaction or transaction group to the network.

## Endpoint

```http
POST /v2/transactions
```

## Query Parameters

| Parameter             | Type    | Required | Description                                                                                                                                                     |
| --------------------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| skip-pq-address-check | boolean | Optional | Skip post-quantum address checks, including the check that rejects PQ authorizer and LogicSig escrow (TEAL v13 or later) whose address is an Edwards25519 curve |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/transactions" \
--header 'Content-Type: application/x-binary' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field | Type   | Description                       |
| ----- | ------ | --------------------------------- |
| txId  | string | encoding of the transaction hash. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
