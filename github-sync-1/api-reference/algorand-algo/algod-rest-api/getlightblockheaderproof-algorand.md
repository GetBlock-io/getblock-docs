---
description: >-
  Example code for the GetLightBlockHeaderProof REST method. Complete guide on
  how to use GetLightBlockHeaderProof REST in GetBlock Web3 documentation.
---

# GetLightBlockHeaderProof - Algorand

Gets a proof for a given light block header inside a state proof commitment.

## Endpoint

```http
GET /v2/blocks/{round}/lightheader/proof
```

## Path Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| round     | integer | Yes      | A round number. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND/lightheader/proof"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field     | Type    | Description                                                                                              |
| --------- | ------- | -------------------------------------------------------------------------------------------------------- |
| index     | integer | The index of the light block header in the vector commitment tree                                        |
| proof     | string  | The encoded proof.                                                                                       |
| treedepth | integer | Represents the depth of the tree that is being proven, i.e. the number of edges from a leaf to the root. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
