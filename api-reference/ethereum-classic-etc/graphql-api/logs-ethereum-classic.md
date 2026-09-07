---
description: Query Ethereum Classic event logs with the GraphQL API.
---

# logs - Ethereum Classic

Returns event logs matching a filter over a block range, addresses, and topics, each with its data, topics, and originating transaction — the GraphQL equivalent of `eth_getLogs`.

## Schema

```graphql
logs(filter: FilterCriteria!): [Log!]!
```

## Arguments

| Argument         | Type            | Description                 |
| ---------------- | --------------- | --------------------------- |
| filter.fromBlock | Long            | Start block of the range    |
| filter.toBlock   | Long            | End block of the range      |
| filter.addresses | \[Address!]     | Contract addresses to match |
| filter.topics    | \[\[Bytes32!]!] | Topic filters               |

## Query

```graphql
query {
  logs(filter: {fromBlock: 21200000, toBlock: 21200010, addresses: ["0x82A618305706B14e7bcf2592D4B9324A366b6dAd"]}) {
    index
    topics
    data
    transaction { hash }
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { logs(filter: {fromBlock: 21200000, toBlock: 21200010, addresses: [\"0x82A618305706B14e7bcf2592D4B9324A366b6dAd\"]}) { index topics data transaction { hash } } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "logs": [
            {
                "index": 0,
                "topics": [
                    "0xddf252ad..."
                ],
                "data": "0x00...",
                "transaction": {
                    "hash": "0x3c8a1f5b9e2d4c7a6b0f1e3d5c8a2b4f6e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7"
                }
            }
        ]
    }
}
```

## Response Fields

| Field       | Type        | Description                          |
| ----------- | ----------- | ------------------------------------ |
| index       | Int         | Log index within the block           |
| topics      | \[Bytes32]  | Indexed event topics                 |
| data        | Bytes       | Non-indexed event data               |
| transaction | Transaction | The transaction that emitted the log |

## Use Cases

* **Event Indexing**: Ingest contract events with nested transaction context
* **Transfer Tracking**: Filter Transfer events for a token
* **Analytics**: Aggregate events over a range

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| errors\[]                 | filter too broad | The filter matches too many logs                  |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
