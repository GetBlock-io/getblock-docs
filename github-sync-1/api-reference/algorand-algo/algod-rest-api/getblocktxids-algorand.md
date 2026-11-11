---
description: >-
  Example code for the GetBlockTxids REST method. Complete guide on how to use
  GetBlockTxids REST in GetBlock Web3 documentation.
---

# GetBlockTxids - Algorand

Get the top level transaction IDs for the block on the given round.

## Endpoint

```http
GET /v2/blocks/{round}/txids
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND/txids"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field      | Type  | Description            |
| ---------- | ----- | ---------------------- |
| blockTxids | array | Block transaction IDs. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
