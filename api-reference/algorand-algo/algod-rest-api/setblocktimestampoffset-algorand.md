---
description: >-
  Example code for the SetBlockTimeStampOffset REST method. Complete guide on
  how to use SetBlockTimeStampOffset REST in GetBlock Web3 documentation.
---

# SetBlockTimeStampOffset - Algorand

Sets the timestamp offset (seconds) for blocks in dev mode. Providing an offset of 0 will unset this value and try to use the real clock for the timestamp.

{% hint style="warning" %}
This is a node-administration, participation, or diagnostic endpoint and is not served on GetBlock shared endpoints. It requires a Dedicated Node.
{% endhint %}

## Endpoint

```
POST /v2/devmode/blocks/offset/{offset}
```

## Path Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| offset    | integer | Yes      | The timestamp offset for blocks in dev mode. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ALGO_ALGOD}v2/devmode/blocks/offset/REPLACE_OFFSET" \
--header 'Content-Type: application/json' \
--data-binary '<request body>'
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field  | Type             | Description |
| ------ | ---------------- | ----------- |
| (body) | application/json | OK          |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
