---
description: >-
  Example code for the Metrics REST method. Complete guide on how to use Metrics
  REST in GetBlock Web3 documentation.
---

# Metrics - Algorand

Return metrics about algod functioning.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /metrics
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}metrics"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description                              |
| ------ | ---------------- | ---------------------------------------- |
| (body) | application/json | text with #-comments and key:value lines |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
