# Rosetta API

The Rosetta API is a standardized interface for reading Cardano data and constructing transactions. It is GetBlock's core Cardano interface and the recommended path for exchange integrations, because the same request and response shapes work across every Rosetta-enabled chain.

Every endpoint is an HTTP `POST` whose body carries a `network_identifier` and endpoint-specific fields. The API has two halves: a Data set for reading chain state, and a Construction set for building, signing, and submitting transactions through an offline-friendly flow.

## Base URL

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. The network identifier for mainnet is `{"blockchain": "cardano", "network": "mainnet"}`.

## Data API

| Endpoint                  | Description                            |
| ------------------------- | -------------------------------------- |
| POST /network/list        | List supported networks                |
| POST /network/status      | Current network status and tip         |
| POST /network/options     | Version and allowed types              |
| POST /block               | Block and its transactions             |
| POST /block/transaction   | A single transaction from a block      |
| POST /account/balance     | Account balance, ada and native assets |
| POST /account/coins       | Unspent outputs for an address         |
| POST /mempool             | Transaction ids in the mempool         |
| POST /mempool/transaction | A single mempool transaction           |

## Construction API

The construction endpoints form an ordered flow: derive, preprocess, metadata, payloads, parse, combine, hash, and submit. Only `metadata` and `submit` require a live network connection; the rest can run offline.

<table data-search="false"><thead><tr><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>POST /construction/derive</td><td>Derive an address from a public key.</td></tr><tr><td>POST /construction/preprocess</td><td>Begin construction from operations.</td></tr><tr><td>POST /construction/metadata</td><td>Fetch online construction metadata.</td></tr><tr><td>POST /construction/payloads</td><td>Build an unsigned transaction and payloads.</td></tr><tr><td>POST /construction/parse</td><td>Parse a transaction back to operations.</td></tr><tr><td>POST /construction/combine</td><td>Combine signatures into a signed transaction.</td></tr><tr><td>POST /construction/hash</td><td>Hash a signed transaction.</td></tr><tr><td>POST /construction/submit</td><td>Broadcast a signed transaction.</td></tr></tbody></table>

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Rosetta API Specification](https://www.rosetta-api.org/docs/welcome.html)
* [Cardano (ADA)](../)
