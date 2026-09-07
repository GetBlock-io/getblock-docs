# GraphQL API

Ethereum Classic exposes an EIP-1767 GraphQL API: a single endpoint (`/graphql`) that lets you request exactly the block, transaction, account, and log fields you need — with nesting — in one query, plus a mutation to broadcast transactions. It complements the JSON-RPC and WebSocket interfaces.

## Endpoint

Send GraphQL over HTTP POST to the endpoint base URL with `graphql` appended (`https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/` + `graphql`).

## Operations

* block — Fetch a block by number or hash
* blocks — Fetch a range of blocks
* transaction — Fetch a transaction by hash
* account — Read an account's balance, nonce, code, and storage
* call — Execute a read-only contract call
* estimateGas — Estimate gas for a call
* logs — Query event logs by filter
* gasPrice — Current gas price
* sendRawTransaction — Broadcast a signed transaction (mutation)

## Support

* Support: [support@getblock.io](mailto:support@getblock.io)
