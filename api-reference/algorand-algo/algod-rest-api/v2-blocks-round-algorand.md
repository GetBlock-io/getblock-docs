---
description: >-
  Example code for the /v2/blocks/{round} REST method. Complete guide on how to
  use the /v2/blocks/{round} REST method in the GetBlock Web3 documentation.
---

# /v2/blocks/{round} - Algorand

Returns the block for a given round, including its certificate and transactions. Use the format query parameter to choose JSON or msgpack.

## Endpoint

```http
GET /v2/blocks/{round}
```

## Path Parameters

| Parameter | Type    | Description          |
| --------- | ------- | -------------------- |
| round     | integer | Round (block) number |

## Query Parameters

| Parameter | Type   | Description               |
| --------- | ------ | ------------------------- |
| format    | string | json (default) or msgpack |

## Example

{% code overflow="wrap" %}
```bash
export ALGO_ALGOD=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${ALGO_ALGOD}v2/blocks/35000000"
```
{% endcode %}

## Response

```json
{
    "block": {
        "rnd": 35000000,
        "prev": "blk-...",
        "seed": "...",
        "ts": 1730000000,
        "txns": [],
        "gh": "genesis-hash",
        "gen": "mainnet-v1.0"
    }
}
```

## Response Fields

| Field      | Type    | Description               |
| ---------- | ------- | ------------------------- |
| block.rnd  | integer | Round number              |
| block.ts   | integer | Block timestamp           |
| block.txns | array   | Transactions in the block |

## Use Cases

* **Block Inspection**: Read a block's transactions
* **Indexing**: Ingest blocks from a live node
* **Explorers**: Render block detail pages

## Error Handling

| Error                     | Message         | Description                                              |
| ------------------------- | --------------- | -------------------------------------------------------- |
| 404 / Not found           | Block not found | The round is above the tip or below the node's retention |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect        |
