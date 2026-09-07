---
description: Broadcast a signed Ethereum Classic transaction with the GraphQL API.
---

# sendRawTransaction - Ethereum Classic

Submits a signed, RLP-encoded transaction and returns its hash — the GraphQL equivalent of `eth_sendRawTransaction`. This is a mutation, not a query.

## Schema

```graphql
mutation { sendRawTransaction(data: Bytes!): Bytes32! }
```

## Arguments

| Argument | Type   | Description                    |
| -------- | ------ | ------------------------------ |
| data     | Bytes! | RLP-encoded signed transaction |

## Query

```graphql
mutation {
  sendRawTransaction(data: "0xf86c808504a817c800...")
}
```

## Example

{% code overflow="wrap" %}
```bash
export ETC_GRAPHQL=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl --location --request POST "${ETC_GRAPHQL}graphql" \
--header 'Content-Type: application/json' \
--data-raw '{"query": "mutation { sendRawTransaction(data: \"0xf86c808504a817c800...\") }"}'
```
{% endcode %}

## Response

```json
{
    "data": {
        "sendRawTransaction": "0x3c8a1f5b9e2d4c7a6b0f1e3d5c8a2b4f6e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7"
    }
}
```

## Response Fields

| Field              | Type    | Description                       |
| ------------------ | ------- | --------------------------------- |
| sendRawTransaction | Bytes32 | Hash of the submitted transaction |

## Use Cases

* **Transaction Submission**: Broadcast a signed transaction over GraphQL
* **Unified Interface**: Submit and query from one GraphQL endpoint
* **Backends**: Send transactions without a second JSON-RPC client

## Error Handling

| Error                     | Message             | Description                                       |
| ------------------------- | ------------------- | ------------------------------------------------- |
| errors\[]                 | invalid transaction | The transaction failed validation                 |
| 403 / RBAC: access denied | Access denied       | The GetBlock access token is missing or incorrect |
