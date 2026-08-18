---
description: >-
  GetBlock provides fast and reliable access to Bitcoin Cash nodes via REST API
  . Connect to the Bitcoin Cash network without running your own infrastructure.
---

# Bitcoin Cash Blockbook (REST) API

The Bitcoin Cash REST API serves indexed blockchain data over HTTP. Each endpoint is a path under a versioned base, queried with path and query parameters, and returns JSON. The API is backed by an address- and xpub-indexed view of the chain, so it answers address, wallet, transaction, block, UTXO, and fiat-rate queries that a plain node cannot.

### Base URL

All endpoints are served under the `/api/v2/` path on the Bitcoin Cash endpoint:

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. Requests are standard HTTP: `GET` for queries and `POST` for transaction broadcasting.

{% hint style="info" %}
The REST API is served by the indexer add-on. Confirm from the GetBlock dashboard that the add-on is enabled on the Bitcoin Cash endpoint, and confirm the exact base path, before relying on the paths below.
{% endhint %}

### Endpoints

| Method | Endpoint                            | Description                                         |
| ------ | ----------------------------------- | --------------------------------------------------- |
| GET    | `/api/v2/address/{address}`         | Balance and transaction data for an address         |
| GET    | `/api/v2/xpub/{xpub}`               | Wallet-level data for an xpub or descriptor         |
| GET    | `/api/v2/utxo/{addressOrXpub}`      | Unspent outputs for an address, xpub, or descriptor |
| GET    | `/api/v2/balancehistory/{address}`  | Aggregated balance history over time                |
| GET    | `/api/v2/tx/{txid}`                 | Normalized transaction by id                        |
| GET    | `/api/v2/tx-specific/{txid}`        | Node-native transaction JSON by id                  |
| GET    | `/api/v2/block/{blockHash}`         | Block by height or hash with its transactions       |
| GET    | `/api/v2/block-index/{blockHeight}` | Block hash at a given height                        |
| GET    | `/api/v2/rawblock/{blockId}`        | Raw serialized block hex                            |
| GET    | `/api/v2/feestats/{blockId}`        | Fee statistics for a block                          |
| GET    | `/api/v2/estimatefee/{blocks}`      | Backend fee estimate for a confirmation target      |
| POST   | `/api/v2/sendtx/`                   | Broadcast a signed raw transaction                  |
| GET    | `/api/v2/tickers/`                  | Current or historical fiat rates                    |
| GET    | `/api/v2/tickers-list/`             | Currencies available at a timestamp                 |
| GET    | `/api/v2/multi-tickers/`            | Fiat rates for multiple timestamps                  |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Blockbook REST API Reference](https://github.com/trezor/blockbook/blob/master/openapi.yaml)
* [Bitcoin Cash (BCH)](../)
* [Blockbook Add-on](../../../add-ons/blockbook.md)
