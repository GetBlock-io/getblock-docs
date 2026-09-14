---
description: >-
  Example code for the GetBlockHash REST method. Complete guide on how to use
  GetBlockHash REST in GetBlock Web3 documentation.
---

# GetBlockHash - Algorand

Get the block hash for the block on the given round.

## Endpoint

```http
GET /v2/blocks/{round}/hash
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND/hash"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field     | Type   | Description        |
| --------- | ------ | ------------------ |
| blockHash | string | Block header hash. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
