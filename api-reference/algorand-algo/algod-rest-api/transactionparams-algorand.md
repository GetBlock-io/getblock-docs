---
description: >-
  Example code for the TransactionParams REST method. Complete guide on how to
  use TransactionParams REST in GetBlock Web3 documentation.
---

# TransactionParams - Algorand

Get parameters for constructing a new transaction.

## Endpoint

```http
GET /v2/transactions/params
```

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/transactions/params"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field             | Type    | Description                                                                                                   |
| ----------------- | ------- | ------------------------------------------------------------------------------------------------------------- |
| consensus-version | string  | ConsensusVersion indicates the consensus protocol version                                                     |
| fee               | integer | Fee is the suggested transaction fee                                                                          |
| genesis-hash      | string  | GenesisHash is the hash of the genesis block.                                                                 |
| genesis-id        | string  | GenesisID is an ID listed in the genesis block.                                                               |
| last-round        | integer | LastRound indicates the last round seen                                                                       |
| min-fee           | integer | The minimum transaction fee (not per byte) required for the txn to validate for the current network protocol. |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
