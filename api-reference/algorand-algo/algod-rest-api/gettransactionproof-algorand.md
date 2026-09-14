---
description: >-
  Example code for the GetTransactionProof REST method. Complete guide on how to
  use GetTransactionProof REST in GetBlock Web3 documentation.
---

# GetTransactionProof - Algorand

Get a proof for a transaction in a block.

## Endpoint

```http
GET /v2/blocks/{round}/transactions/{txid}/proof
```

## Path Parameters

| Parameter | Type    | Required | Description                                       |
| --------- | ------- | -------- | ------------------------------------------------- |
| round     | integer | Yes      | A round number.                                   |
| txid      | string  | Yes      | The transaction ID for which to generate a proof. |

## Query Parameters

| Parameter | Type   | Required | Description                                                                                               |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------------- |
| hashtype  | string | Optional | The type of hash function used to create the proof, must be one of:                                       |
| format    | string | Optional | Configures whether the response object is JSON or MessagePack encoded. If not provided, defaults to JSON. |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/REPLACE_ROUND/transactions/REPLACE_TXID/proof"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field     | Type    | Description                                                                                              |
| --------- | ------- | -------------------------------------------------------------------------------------------------------- |
| hashtype  | string  | The type of hash function used to create the proof, must be one of:                                      |
| idx       | integer | Index of the transaction in the block's payset.                                                          |
| proof     | string  | Proof of transaction membership.                                                                         |
| stibhash  | string  | Hash of SignedTxnInBlock for verifying proof.                                                            |
| treedepth | integer | Represents the depth of the tree that is being proven, i.e. the number of edges from a leaf to the root. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
