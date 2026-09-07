---
description: Query an Ethereum Classic transaction with the GraphQL API.
---

# transaction - Ethereum Classic

Returns a transaction by its hash, including sender, recipient, value, gas, status, and — when selected — its receipt logs, in a single query.

## Schema

```graphql
transaction(hash: Bytes32!): Transaction
```

## Arguments

| Argument | Type     | Description      |
| -------- | -------- | ---------------- |
| hash     | Bytes32! | Transaction hash |

## Query

```graphql
query {
  transaction(hash: "0x3c8a1f5b9e2d4c7a6b0f1e3d5c8a2b4f6e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7") {
    hash
    from { address }
    to { address }
    value
    gasUsed
    status
    block { number }
  }
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "query { transaction(hash: \"0x3c8a1f5b9e2d4c7a6b0f1e3d5c8a2b4f6e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7\") { hash from { address } to { address } value gasUsed status block { number } } }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "transaction": {
            "hash": "0x3c8a1f5b9e2d4c7a6b0f1e3d5c8a2b4f6e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7",
            "from": {
                "address": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
            },
            "to": {
                "address": "0x82A618305706B14e7bcf2592D4B9324A366b6dAd"
            },
            "value": "1000000000000000000",
            "gasUsed": 21000,
            "status": 1,
            "block": {
                "number": 21200000
            }
        }
    }
}
```

## Response Fields

| Field  | Type    | Description               |
| ------ | ------- | ------------------------- |
| from   | Account | Sender account            |
| to     | Account | Recipient account         |
| value  | BigInt  | Value transferred in wei  |
| status | Long    | 1 on success, 0 on revert |

## Use Cases

* **Status Checks**: Confirm a transaction succeeded
* **Wallet History**: Render transaction detail with nested block
* **Receipts**: Select logs alongside the transaction

## Error Handling

| Error                     | Message               | Description                                       |
| ------------------------- | --------------------- | ------------------------------------------------- |
| errors\[]                 | transaction not found | No transaction matches the hash                   |
| 403 / RBAC: access denied | Access denied         | The GetBlock access token is missing or incorrect |
