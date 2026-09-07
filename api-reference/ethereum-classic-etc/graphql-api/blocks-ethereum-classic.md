---
description: Query Ethereum Classic blocks with the GraphQL API.
---

# blocks - Ethereum Classic

Returns all blocks in an inclusive range from a starting number to an ending number (or the latest), each with the fields you select. Useful for batch indexing.

## Schema

```graphql
blocks(from: Long!, to: Long): [Block!]!
```

## Arguments

| Argument | Type  | Description                                        |
| -------- | ----- | -------------------------------------------------- |
| from     | Long! | First block number (inclusive)                     |
| to       | Long  | Last block number (inclusive); omit for the latest |

## Query

```graphql
query {
  blocks(from: 21200000, to: 21200005) {
    number
    transactionCount
    gasUsed
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { blocks(from: 21200000, to: 21200005) { number transactionCount gasUsed } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "blocks": [
            {
                "number": 21200000,
                "transactionCount": 12,
                "gasUsed": 1200000
            },
            {
                "number": 21200001,
                "transactionCount": 8,
                "gasUsed": 800000
            }
        ]
    }
}
```

## Response Fields

| Field    | Type  | Description                                              |
| -------- | ----- | -------------------------------------------------------- |
| \[Block] | array | Blocks in the requested range, each with selected fields |

## Use Cases

* **Batch Indexing**: Ingest a block range in one request
* **Backfill**: Replay history for an indexer
* **Analytics**: Aggregate metrics across blocks

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| errors\[]                 | range too large | The requested range exceeds the node's limit      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
