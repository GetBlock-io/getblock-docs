---
description: >-
  Example code for the /v2/blocks/{round-number} REST method. Complete guide on
  how to use the /v2/blocks/{round-number} REST method in the GetBlock Web3
  documentation.
---

# /v2/blocks/{round-number} - Algorand

Returns a block by round from the Indexer, with its header fields and, optionally, its transactions. Archival, so historical blocks are available.

## Endpoint

```http
GET /v2/blocks/{round-number}
```

## Path Parameters

| Parameter    | Type    | Description          |
| ------------ | ------- | -------------------- |
| round-number | integer | Round (block) number |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_INDEXER=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_INDEXER}v2/blocks/35000000"
```
{% endcode %}

## Response

```json
{
    "round": 35000000,
    "timestamp": 1730000000,
    "genesis-id": "mainnet-v1.0",
    "previous-block-hash": "blk-...",
    "transactions-root": "...",
    "txn-counter": 45000000
}
```

## Response Fields

| Field       | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| round       | integer | Round number                   |
| timestamp   | integer | Block timestamp                |
| txn-counter | integer | Cumulative transaction counter |

## Use Cases

* **Block Reads**: Fetch a historical block
* **Indexing**: Ingest block metadata
* **Explorers**: Render block pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / Not found           | Not found     | No block at that round                            |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
