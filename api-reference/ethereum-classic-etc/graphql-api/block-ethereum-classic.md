---
description: Query a single Ethereum Classic block with the GraphQL API.
---

# block - Ethereum Classic

Returns a block by its number or hash (or the latest block when neither is given), letting you select exactly the header fields and nested transactions you need in one request.

## Schema

```graphql
block(number: Long, hash: Bytes32): Block
```

## Arguments

| Argument | Type    | Description                                 |
| -------- | ------- | ------------------------------------------- |
| number   | Long    | Block number; omit for the latest block     |
| hash     | Bytes32 | Block hash (mutually exclusive with number) |

## Query

```graphql
query {
  block(number: 21200000) {
    number
    hash
    timestamp
    gasUsed
    gasLimit
    miner { address }
    transactionCount
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { block(number: 21200000) { number hash timestamp gasUsed gasLimit miner { address } transactionCount } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "block": {
            "number": 21200000,
            "hash": "0x1a2b...",
            "timestamp": 1730000000,
            "gasUsed": 1200000,
            "gasLimit": 8000000,
            "miner": {
                "address": "0x82A618305706B14e7bcf2592D4B9324A366b6dAd"
            },
            "transactionCount": 12
        }
    }
}
```

## Response Fields

| Field            | Type    | Description                         |
| ---------------- | ------- | ----------------------------------- |
| number           | Long    | Block number                        |
| hash             | Bytes32 | Block hash                          |
| miner            | Account | Block producer account              |
| transactionCount | Int     | Number of transactions in the block |

## Use Cases

* **Selective Reads**: Fetch only the block fields you need
* **Explorers**: Render block pages with nested data
* **Indexing**: Pull block + transactions in one round trip

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| errors\[]                 | block not found | No block matches the number or hash               |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
