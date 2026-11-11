---
description: >-
  Example code for the lookupBlock REST method. Complete guide on how to use
  lookupBlock REST in GetBlock Web3 documentation.
---

# lookupBlock - Algorand

Lookup block.

## Endpoint

```http
GET /v2/blocks/{round-number}
```

## Path Parameters

| Parameter    | Type    | Required | Description  |
| ------------ | ------- | -------- | ------------ |
| round-number | integer | Yes      | Round number |

## Query Parameters

| Parameter   | Type    | Required | Description                                                                                  |
| ----------- | ------- | -------- | -------------------------------------------------------------------------------------------- |
| header-only | boolean | Optional | Header only flag. When this is set to true, returned block does not contain the transactions |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/blocks/REPLACE_ROUND-NUMBER"
```
{% endcode %}

## Response

Returns `application/json`. Top-level fields:

| Field                    | Type    | Description                                                                                                                                                      |
| ------------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bonus                    | integer | the potential bonus payout for this block.                                                                                                                       |
| congestion-tax           | integer | the fee required, beyond the minimum fee, for "normal" transactions in this block.                                                                               |
| fees-collected           | integer | the sum of all fees paid by transactions in this block.                                                                                                          |
| genesis-hash             | string  | \[gh] hash to which this block belongs.                                                                                                                          |
| genesis-id               | string  | \[gen] ID to which this block belongs.                                                                                                                           |
| load                     | integer | the degree to which this block is full, based on the number of bytes in the final block compared to the maximum allowed. Expressed as a fixed-point integer with |
| participation-updates    | object  | Participation account data that needs to be checked/acted on by the network.                                                                                     |
| previous-block-hash      | string  | \[prev] Previous block hash.                                                                                                                                     |
| previous-block-hash-512  | string  | \[prev512] Previous block hash, using SHA-512.                                                                                                                   |
| proposer                 | string  | the proposer of this block.                                                                                                                                      |
| proposer-payout          | integer | the actual amount transferred to the proposer from the fee sink.                                                                                                 |
| rewards                  | object  | Fields relating to rewards,                                                                                                                                      |
| round                    | integer | \[rnd] Current round on which this block was appended to the chain.                                                                                              |
| seed                     | string  | \[seed] Sortition seed.                                                                                                                                          |
| state-proof-tracking     | array   | Tracks the status of state proofs.                                                                                                                               |
| timestamp                | integer | \[ts] Block creation timestamp in seconds since eposh                                                                                                            |
| transactions             | array   | \[txns] list of transactions corresponding to a given round.                                                                                                     |
| transactions-root        | string  | \[txn] TransactionsRoot authenticates the set of transactions appearing in the block. More specifically, it's the root of a merkle tree whose leaves are the bl  |
| transactions-root-sha256 | string  | \[txn256] TransactionsRootSHA256 is an auxiliary TransactionRoot, built using a vector commitment instead of a merkle tree, and SHA256 hash function instead of  |
| transactions-root-sha512 | string  | \[txn512] TransactionsRootSHA512 is an auxiliary TransactionRoot, built using a vector commitment instead of a merkle tree, and SHA512 hash function instead of  |
| txn-counter              | integer | \[tc] TxnCounter counts the number of transactions committed in the ledger, from the time at which support for this feature was introduced.                      |
| upgrade-state            | object  | Fields relating to a protocol upgrade.                                                                                                                           |
| upgrade-vote             | object  | Fields relating to voting for a protocol upgrade.                                                                                                                |

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400                       | Bad request   | A parameter was missing or malformed              |
| 404                       | Not found     | The requested resource does not exist             |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
