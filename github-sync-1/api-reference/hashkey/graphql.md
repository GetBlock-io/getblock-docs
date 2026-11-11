---
description: >-
  Example code for the HashKey Chain GraphQL API (EIP-1767). Complete guide on
  how to query blocks, transactions, accounts, and logs with GraphQL in GetBlock
  Web3 documentation.
---

# GraphQL - HashKey Chain

HashKey Chain exposes an EIP-1767 GraphQL API: a single `/graphql` endpoint that lets you request exactly the block, transaction, account, and log fields you need — with nesting — in one query, plus a mutation to broadcast transactions. It complements the JSON-RPC and WebSocket interfaces, and is useful when a single round-trip should return a precisely-shaped slice of chain data.

## Endpoint

GraphQL is served over HTTP POST at the endpoint base URL with `graphql` appended.

{% code overflow="wrap" %}
```bash
export HSK_GQL=https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/

# POST a GraphQL query to the /graphql path
curl -X POST "${HSK_GQL}graphql" \
  -H "Content-Type: application/json" \
  -d '{"query":"{ block { number hash timestamp gasUsed } }"}'
```
{% endcode %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. HashKey Chain is served from the New York region; GraphQL shares the same base URL as JSON-RPC, with `graphql` appended.
{% endhint %}

## Operations

<table data-search="false"><thead><tr><th>Operation</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>block</td><td>query</td><td>Fetch a block by number or hash</td></tr><tr><td>blocks</td><td>query</td><td>Fetch a range of blocks (<code>from</code> / <code>to</code>)</td></tr><tr><td>transaction</td><td>query</td><td>Fetch a transaction by hash</td></tr><tr><td>account</td><td>query</td><td>Read an account's balance, nonce, code, and storage at a block</td></tr><tr><td>call</td><td>query</td><td>Execute a read-only contract call</td></tr><tr><td>estimateGas</td><td>query</td><td>Estimate gas for a call</td></tr><tr><td>logs</td><td>query</td><td>Query event logs by filter (addresses, topics, block range)</td></tr><tr><td>gasPrice</td><td>query</td><td>Current gas price</td></tr><tr><td>sendRawTransaction</td><td>mutation</td><td>Broadcast a signed raw transaction</td></tr></tbody></table>

## Example Queries

### Block with its transactions

{% code overflow="wrap" %}
```graphql
{
  block(number: 5000000) {
    number
    hash
    timestamp
    gasUsed
    gasLimit
    transactions {
      hash
      from { address }
      to { address }
      value
    }
  }
}
```
{% endcode %}

### Account balance and nonce

{% code overflow="wrap" %}
```graphql
{
  account(address: "0x177b1a2c3d4e5f60718293a4b5c6d7e8f90a1b2c") {
    balance
    transactionCount
    code
  }
}
```
{% endcode %}

### Logs by filter

{% code overflow="wrap" %}
```graphql
{
  logs(filter: {
    fromBlock: 5000000,
    toBlock: 5000100,
    addresses: ["0x177b1a2c3d4e5f60718293a4b5c6d7e8f90a1b2c"]
  }) {
    index
    topics
    data
    transaction { hash }
  }
}
```
{% endcode %}

### Read-only contract call

{% code overflow="wrap" %}
```graphql
{
  call(data: {
    to: "0x177b1a2c3d4e5f60718293a4b5c6d7e8f90a1b2c",
    data: "0x18160ddd"
  }) {
    data
    status
    gasUsed
  }
}
```
{% endcode %}

### Broadcast a transaction (mutation)

{% code overflow="wrap" %}
```graphql
mutation {
  sendRawTransaction(data: "0x02f8...signed-tx-hex")
}
```
{% endcode %}

## Response

A GraphQL response wraps the requested shape under a top-level `data` key:

```json
{
    "data": {
        "block": {
            "number": 5000000,
            "hash": "0x9a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f9",
            "timestamp": "0x66a1b2c3",
            "gasUsed": "0x5208"
        }
    }
}
```

## Response Fields

| Field  | Type   | Description                                                              |
| ------ | ------ | ------------------------------------------------------------------------ |
| data   | object | The result, shaped exactly like the query                                |
| errors | array  | Present only on failure; each entry has a `message` and the query `path` |

## Use Cases

* **Precise fetching**: Request only the block, transaction, or account fields you need, nested in one round-trip
* **Explorer backends**: Pull a block with its transactions and logs in a single query
* **Batch reads**: Fetch a range of blocks (`blocks(from, to)`) without many JSON-RPC calls
* **Broadcast**: Submit a signed transaction with the `sendRawTransaction` mutation

## Error Handling

| Error                     | Message                   | Description                                                                 |
| ------------------------- | ------------------------- | --------------------------------------------------------------------------- |
| 400 / Bad request         | Syntax / validation error | The GraphQL query was malformed or referenced unknown fields (see `errors`) |
| 200 with `errors`         | Field resolution error    | The query parsed but a field could not be resolved (e.g. block not found)   |
| 403 / RBAC: access denied | Access denied             | The GetBlock access token is missing or incorrect                           |

## See Also

* [HashKey Chain overview](./) — network information, interfaces, and quickstart
* [EIP-1767: GraphQL interface to Ethereum nodes](https://eips.ethereum.org/EIPS/eip-1767)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
