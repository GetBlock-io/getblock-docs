---
description: >-
  Example code for the GetStateProof REST method. Complete guide on how to use
  GetStateProof REST in GetBlock Web3 documentation.
---

# GetStateProof - Algorand

Get a state proof that covers a given round.

## Endpoint

```http
GET /v2/stateproofs/{round}
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/stateproofs/REPLACE_ROUND"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field      | Type   | Description                                                    |
| ---------- | ------ | -------------------------------------------------------------- |
| Message    | object | Represents the message that the state proofs are attesting to. |
| StateProof | string | The encoded StateProof for the message.                        |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
