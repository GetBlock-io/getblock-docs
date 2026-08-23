---
description: >-
  GetBlock provides fast and reliable access to Bitcoin nodes via Blockbook API.
  Connect to the Bitcoin network without running your own infrastructure.
---

# Bitcoin Blockbook API

Blockbook is an address-indexed and xpub-indexed API for Bitcoin. A standard Bitcoin node tracks unspent outputs but does not organize them by address, so it cannot answer address-level questions on its own. Blockbook, built by Trezor, maintains that index on top of the chain and answers questions about an address or a whole wallet: balances, transaction history, and unspent outputs.

The API is served in two interchangeable forms. A [REST](../bitcoin-blockbook-rest-api/) interface exposes each query as an HTTP path with query parameters, and a JSON-RPC interface exposes the same queries as `bb_`-prefixed methods. Both return the same indexed data. Each method page below shows both forms.

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```

### Available Methods

| Method                | Description                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| bb\_getBlock          | Returns a block by height or hash, including its metadata and a paged list of the transactions it contains                                 |
| bb\_getBlockHash      | Returns the block hash at a given block height                                                                                             |
| bb\_getTx             | Returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data in the indexer's unified schema |
| bb\_getTxSpecific     | Returns the transaction exactly as the bitcoin node reports it, in the node's own json shape rather than the indexer's normalized schema   |
| bb\_getAddress        | Returns balance and transaction data for a single address                                                                                  |
| bb\_getXpub           | Returns wallet-level balance and transaction data for an extended public key or output descriptor                                          |
| bb\_getUTXOs          | Returns the unspent transaction outputs for an address, extended public key, or descriptor                                                 |
| bb\_getBalanceHistory | Returns aggregated balance-change history for an address, extended public key, or descriptor over a time range                             |
| bb\_sendTransaction   | Broadcasts a signed, serialized transaction to the network through the indexer and returns its transaction id                              |
| bb\_getTickers        | Returns fiat exchange rates for the coin at a given timestamp, or the latest rates when no timestamp is supplied                           |
| bb\_getTickersList    | Returns the list of fiat currencies for which rates are available at a given timestamp                                                     |
