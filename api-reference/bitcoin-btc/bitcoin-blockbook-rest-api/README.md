---
description: >-
  GetBlock provides fast and reliable access to Bitcoin nodes via REST API.
  Connect to the Bitcoin network without running your own infrastructure.
---

# Bitcoin Blockbook REST API

The Bitcoin Blockbook REST API serves indexed blockchain data over HTTP. Each endpoint is a path under `/api/v2/`, queried with path and query parameters, and returns JSON. It is the REST form of the same indexed data exposed by the [Blockbook `bb_` JSON-RPC methods](../botcoin-blockbook-api/).

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

### Endpoints

<table data-search="false"><thead><tr><th>Method</th><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/v2/address/{address}</code></td><td>Returns balance and transaction data for a single bitcoin address</td></tr><tr><td>GET</td><td><code>/api/v2/xpub/{xpub}</code></td><td>Returns wallet-level balance and transaction data for an extended public key or output descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/utxo/{addressOrXpub}</code></td><td>Returns the unspent transaction outputs for an address, extended public key, or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/balancehistory/{address}</code></td><td>Returns balance and transaction data for a single bitcoin address</td></tr><tr><td>GET</td><td><code>/api/v2/tx/{txid}</code></td><td>Returns a normalized transaction by its id, with inputs, outputs, addresses, and confirmation data in the indexer's unified schema</td></tr><tr><td>GET</td><td><code>/api/v2/tx-specific/{txid}</code></td><td>Returns a normalized transaction by its id, with inputs, outputs, addresses, and confirmation data in the indexer's unified schema</td></tr><tr><td>GET</td><td><code>/api/v2/block/{blockHash}</code></td><td>Returns a block by height or hash, including its metadata and a paged list of the transactions it contains</td></tr><tr><td>GET</td><td><code>/api/v2/block-index/{blockHeight}</code></td><td>Returns the block hash at a given block height</td></tr><tr><td>GET</td><td><code>/api/v2/rawblock/{blockId}</code></td><td>Returns the raw serialized hex of a block, selected by height or hash</td></tr><tr><td>GET</td><td><code>/api/v2/feestats/{blockId}</code></td><td>Returns the raw serialized hex of a block, selected by height or hash</td></tr><tr><td>GET</td><td><code>/api/v2/estimatefee/{blocks}</code></td><td>Returns the backend fee estimate for a target number of blocks to confirmation</td></tr><tr><td>POST</td><td><code>/api/v2/sendtx/</code></td><td>Broadcasts a signed, serialized transaction to the bitcoin network through the backend node and returns the transaction id on acceptance</td></tr><tr><td>GET</td><td><code>/api/v2/tickers/</code></td><td>Returns current or historical fiat exchange rates for bitcoin</td></tr><tr><td>GET</td><td><code>/api/v2/tickers-list/</code></td><td>Returns the fiat currencies for which the indexer has rate data available at a given timestamp</td></tr><tr><td>GET</td><td><code>/api/v2/multi-tickers/</code></td><td>Returns fiat rate tickers for a comma-separated list of unix timestamps</td></tr></tbody></table>
