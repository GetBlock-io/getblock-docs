---
description: >-
  GetBlock provides fast and reliable access to Bitcoin address, UTXO, and
  wallet data via the Blockbook REST API. Connect to the Bitcoin network without
  running your own infrastructure.
---

# Bitcoin Blockbook REST API

Blockbook is an address-indexed and xpub-indexed view of the Bitcoin chain. A standard Bitcoin node tracks unspent outputs but does not organize them by address, so it cannot answer address-level questions on its own. Blockbook, built by Trezor, maintains that index on top of the chain and answers questions about an address or a whole wallet: balances, transaction history, and unspent outputs.

GetBlock's Blockbook add-on is provisioned through separate REST and WebSocket endpoints. Use REST for address, transaction, balance, and UTXO queries. Use [WebSocket](../bitcoin-blockbook-websocket-api/) when subscriptions are required. A standard Bitcoin JSON-RPC endpoint does not support Blockbook methods.

{% hint style="warning" %}
**A Bitcoin Core JSON-RPC access token and a Blockbook REST access token are different endpoint configurations.** They are provisioned separately and have different URLs. Sending Blockbook requests to a standard Bitcoin JSON-RPC endpoint will not work, and calling `bb_`-prefixed method names over JSON-RPC is not supported on any GetBlock endpoint.
{% endhint %}

### Setting up a Blockbook endpoint

{% stepper %}
{% step %}
Sign in to [GetBlock](https://account.getblock.io) and start a new endpoint in the configurator
{% endstep %}

{% step %}
Select **Bitcoin → Mainnet → Full → Blockbook(REST)**.&#x20;

<figure><img src="../../../.gitbook/assets/Screenshot 2026-09-18 at 4.41.24 AM.png" alt="Bitcoin Blockbook(REST) API"><figcaption></figcaption></figure>

{% hint style="info" %}
Do not use the standard Bitcoin JSON-RPC API for address-indexed queries — it has no address index and does not serve `/api/v2/` paths
{% endhint %}
{% endstep %}

{% step %}
Use the access token the configurator returns. Blockbook gets its own endpoint URL, distinct from any Bitcoin JSON-RPC token already on the account, and every path below is appended to that URL
{% endstep %}
{% endstepper %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

### Endpoints

<table data-search="false"><thead><tr><th>Method</th><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/status</code></td><td>Returns the indexer's sync state and the connected backend node's metadata</td></tr><tr><td>GET</td><td><code>/api/v2/address/{address}</code></td><td>Returns balance and transaction data for a single bitcoin address</td></tr><tr><td>GET</td><td><code>/api/v2/xpub/{xpub}</code></td><td>Returns wallet-level balance and transaction data for an extended public key or output descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/utxo/{addressOrXpub}</code></td><td>Returns the unspent transaction outputs for an address, extended public key, or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/balancehistory/{address}</code></td><td>Returns aggregated balance-change history for an address, extended public key, or descriptor over a time range</td></tr><tr><td>GET</td><td><code>/api/v2/tx/{txid}</code></td><td>Returns a normalized transaction by its id, with inputs, outputs, addresses, and confirmation data in the indexer's unified schema</td></tr><tr><td>GET</td><td><code>/api/v2/tx-specific/{txid}</code></td><td>Returns the transaction exactly as the bitcoin node reports it, in the node's own json shape rather than the indexer's normalized schema</td></tr><tr><td>GET</td><td><code>/api/v2/block/{blockHash}</code></td><td>Returns a block by height or hash, including its metadata and a paged list of the transactions it contains</td></tr><tr><td>GET</td><td><code>/api/v2/block-index/{blockHeight}</code></td><td>Returns the block hash at a given block height</td></tr><tr><td>GET</td><td><code>/api/v2/rawblock/{blockId}</code></td><td>Returns the raw serialized hex of a block, selected by height or hash</td></tr><tr><td>GET</td><td><code>/api/v2/feestats/{blockId}</code></td><td>Returns fee statistics for the transactions contained in one block</td></tr><tr><td>GET</td><td><code>/api/v2/estimatefee/{blocks}</code></td><td>Returns the backend fee estimate for a target number of blocks to confirmation</td></tr><tr><td>POST</td><td><code>/api/v2/sendtx/</code></td><td>Broadcasts a signed, serialized transaction to the bitcoin network through the backend node and returns the transaction id on acceptance</td></tr><tr><td>GET</td><td><code>/api/v2/tickers/</code></td><td>Returns current or historical fiat exchange rates for bitcoin</td></tr><tr><td>GET</td><td><code>/api/v2/tickers-list/</code></td><td>Returns the fiat currencies for which the indexer has rate data available at a given timestamp</td></tr><tr><td>GET</td><td><code>/api/v2/multi-tickers/</code></td><td>Returns fiat rate tickers for a comma-separated list of unix timestamps</td></tr></tbody></table>

### Wallet RPC methods are not available

`listunspent`, `listtransactions`, `getbalance`, and the other Bitcoin Core **wallet** RPC methods operate on a wallet loaded on the node itself. Shared nodes carry no user wallets, so these methods are disabled and return `405 Method Not Allowed`. Blockbook is the supported route to the same data:

| Bitcoin Core wallet method | Blockbook REST equivalent                 |
| -------------------------- | ----------------------------------------- |
| `listunspent`              | `/api/v2/utxo/{address}`                  |
| `listtransactions`         | `/api/v2/address/{address}?details=txs`   |
| `getbalance`               | `/api/v2/address/{address}?details=basic` |
| `getreceivedbyaddress`     | `/api/v2/address/{address}?details=basic` |

### Troubleshooting

| Symptom                                                             | Cause                                                                                                         | Fix                                                                               |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `-32603 Internal error`, `upstream request failed` on a `bb_*` call | A Blockbook method name was sent over JSON-RPC to a standard Bitcoin endpoint, which does not know the method | Use the REST path from the mapping table above, against a Blockbook REST endpoint |
| `405 Method Not Allowed` on `listunspent` or another wallet method  | Bitcoin Core wallet RPCs are disabled on shared nodes                                                         | Use `/api/v2/utxo/{address}`                                                      |
| An HTML page is returned instead of JSON                            | A POST was sent to the endpoint root, which serves the Blockbook web interface                                | Send a GET to a `/api/v2/` path                                                   |
| `403 Forbidden`                                                     | The access token is missing, invalid, or belongs to a different endpoint configuration                        | Confirm the token was provisioned as Bitcoin → Mainnet → Full → Blockbook(REST)   |
