---
description: >-
  Example code for the GetBlockTimeStampOffset REST method. Complete guide on
  how to use GetBlockTimeStampOffset REST in GetBlock Web3 documentation.
---

# GetBlockTimeStampOffset - Algorand

Gets the current timestamp offset.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```http
GET /v2/devmode/blocks/offset
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/devmode/blocks/offset"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type    | Description                  |
| ------ | ------- | ---------------------------- |
| offset | integer | Timestamp offset in seconds. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
