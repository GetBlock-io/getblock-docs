---
description: >-
  GetBlock provides fast and reliable access to Litecoin address, UTXO, and
  wallet data via the Blockbook REST API. Connect to the Litecoin network
  without running your own infrastructure.
---

# Litecoin Blockbook REST API

Blockbook is an address-indexed and xpub-indexed view of the Litecoin chain. A Litecoin Core node tracks unspent outputs but does not organize them by address, so it cannot answer address-level questions on its own. Blockbook, built by Trezor, maintains that index on top of the chain and answers questions about an address or a whole wallet: balances, transaction history, and unspent outputs.

GetBlock's Blockbook add-on is provisioned through separate REST and WebSocket endpoints. Use REST for address, transaction, balance, and UTXO queries. Use [WebSocket](../litecoin-blockbook-websocket-api/) when subscriptions are required. A standard Litecoin JSON-RPC endpoint does not support Blockbook methods.

{% hint style="warning" %}
**A Litecoin Core JSON-RPC access token and a Blockbook REST access token are different endpoint configurations.** They are provisioned separately and have different URLs. The Litecoin Core **wallet** RPCs, including `listunspent`, `listtransactions`, and `getbalance`, operate on a wallet loaded on the node itself; shared nodes carry no user wallets, so those methods are disabled. Blockbook is the supported route to the same data.
{% endhint %}

### Setting up a Blockbook endpoint

{% stepper %}
{% step %}
Sign in to [GetBlock](https://account.getblock.io) and start a new endpoint in the configurator
{% endstep %}

{% step %}
Select **Litecoin → Mainnet → Full → Blockbook → REST**. Do not use the standard Litecoin JSON-RPC API for address-indexed queries — it has no address index and does not serve `/api/v2/` paths
{% endstep %}

{% step %}
Use the access token the configurator returns. Blockbook gets its own endpoint URL, distinct from any Litecoin JSON-RPC token already on the account, and every path below is appended to that URL
{% endstep %}
{% endstepper %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

### Endpoints

<table data-search="false"><thead><tr><th>Method</th><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/status</code></td><td>Returns the indexer's sync state and the connected node's metadata</td></tr><tr><td>GET</td><td><code>/api/v2/address/{address}</code></td><td>Returns balance and transaction data for a single Litecoin address</td></tr><tr><td>GET</td><td><code>/api/v2/xpub/{xpub}</code></td><td>Returns wallet-level balance and transaction data for an extended public key or output descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/utxo/{addressOrXpub}</code></td><td>Returns the unspent transaction outputs for an address, extended public key, or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/balancehistory/{address}</code></td><td>Returns aggregated balance-change history over a time range</td></tr><tr><td>GET</td><td><code>/api/v2/tx/{txid}</code></td><td>Returns a normalized transaction by its id</td></tr><tr><td>GET</td><td><code>/api/v2/tx-specific/{txid}</code></td><td>Returns the transaction in the node's own JSON shape, including MWEB flags</td></tr><tr><td>GET</td><td><code>/api/v2/block/{blockId}</code></td><td>Returns a block by height or hash with a paged list of its transactions</td></tr><tr><td>GET</td><td><code>/api/v2/block-index/{blockHeight}</code></td><td>Returns the block hash at a given block height</td></tr><tr><td>GET</td><td><code>/api/v2/rawblock/{blockId}</code></td><td>Returns the raw serialized hex of a block</td></tr><tr><td>GET</td><td><code>/api/v2/feestats/{blockId}</code></td><td>Returns fee statistics for the transactions in one block</td></tr><tr><td>GET</td><td><code>/api/v2/estimatefee/{blocks}</code></td><td>Returns the backend fee estimate for a confirmation target</td></tr><tr><td>POST</td><td><code>/api/v2/sendtx/</code></td><td>Broadcasts a signed, serialized transaction and returns its transaction id</td></tr><tr><td>GET</td><td><code>/api/v2/tickers/</code></td><td>Returns current or historical fiat exchange rates for Litecoin</td></tr><tr><td>GET</td><td><code>/api/v2/tickers-list/</code></td><td>Returns the currencies with rate data at a given timestamp</td></tr><tr><td>GET</td><td><code>/api/v2/multi-tickers/</code></td><td>Returns fiat rate tickers for several timestamps at once</td></tr></tbody></table>

### Quick check

This request returns the confirmed unspent outputs of an address and confirms that a Blockbook REST endpoint is configured correctly:

{% code overflow="wrap" %}
```bash
curl --location 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/utxo/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?confirmed=true'
```
{% endcode %}

### Litecoin specifics

* **Amounts are in litoshis.** Every balance, value, and fee in the normalized schema is an integer string in the chain's smallest unit, 1e-8 LTC. The node-native `tx-specific` endpoint reports LTC decimals instead.
* **Derivation uses BIP44 coin type 2.** Litecoin account paths read `m/44'/2'/0'/...`, and derived address rows on an xpub query carry that path.
* **MWEB.** Litecoin's MimbleWimble Extension Blocks hold confidential outputs. `tx-specific` marks these with `ismweb` on each input and output; the normalized schema has no equivalent field. See [api/v2/tx-specific](api-v2-tx-specific-litecoin.md).
* **Address formats.** Legacy (`L`), P2SH (`M` or `3`), and bech32 (`ltc1`) addresses are all accepted wherever an address is taken.

### Wallet RPC methods are not available

`listunspent`, `listtransactions`, `getbalance`, and the other Litecoin Core **wallet** RPC methods are disabled on shared nodes. Blockbook is the supported route to the same data:

| Litecoin Core wallet method | Blockbook REST equivalent                 |
| --------------------------- | ----------------------------------------- |
| `listunspent`               | `/api/v2/utxo/{address}`                  |
| `listtransactions`          | `/api/v2/address/{address}?details=txs`   |
| `getbalance`                | `/api/v2/address/{address}?details=basic` |
| `getreceivedbyaddress`      | `/api/v2/address/{address}?details=basic` |

### Troubleshooting

| Symptom                                            | Cause                                                                          | Fix                                                             |
| -------------------------------------------------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `405 Method Not Allowed` on `listunspent`          | Litecoin Core wallet RPCs are disabled on shared nodes                          | Use `/api/v2/utxo/{address}`                                    |
| An HTML page is returned instead of JSON           | A POST was sent to the endpoint root, which serves the Blockbook web interface  | Send a GET to a `/api/v2/` path                                 |
| `403 Forbidden`                                    | The token is missing, invalid, or belongs to a different endpoint configuration | Confirm the token was provisioned as Litecoin → Blockbook → REST |
| An address query returns no `address` or `path`    | Those fields are returned only for xpub and descriptor queries                  | Expected behaviour; the owning address is the one requested     |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
