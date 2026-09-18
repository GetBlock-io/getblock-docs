---
description: >-
  GetBlock provides fast and reliable access to Dogecoin address, UTXO, and
  wallet data via the Blockbook REST API. Connect to the Dogecoin network
  without running your own infrastructure.
---

# Dogecoin Blockbook REST API

Blockbook is an address-indexed and xpub-indexed view of the Dogecoin chain. A Dogecoin Core node tracks unspent outputs but does not organize them by address, so it cannot answer address-level questions on its own. Blockbook, built by Trezor, maintains that index on top of the chain and answers questions about an address or a whole wallet: balances, transaction history, and unspent outputs.

GetBlock's Blockbook add-on is provisioned through separate REST and WebSocket endpoints. Use REST for address, transaction, balance, and UTXO queries. Use [WebSocket](../dogecoin-blockbook-websocket-api/) when subscriptions are required. A standard Dogecoin JSON-RPC endpoint does not support Blockbook methods.

{% hint style="warning" %}
**A Dogecoin Core JSON-RPC access token and a Blockbook REST access token are different endpoint configurations.** They are provisioned separately and have different URLs. Dogecoin Core's **wallet** RPCs — `listunspent`, `listtransactions`, `getbalance`, `sendtoaddress` and the rest — operate on a wallet loaded on the node itself; shared nodes carry no user wallets, so those methods are disabled and are not documented in the [JSON-RPC reference](../dogecoin-json-rpc-api/). Blockbook is the supported route to the same data.
{% endhint %}

### Setting up a Blockbook endpoint

{% stepper %}
{% step %}
Sign in to [GetBlock](https://account.getblock.io) and start a new endpoint in the configurator
{% endstep %}

{% step %}
Select **Dogecoin → Mainnet → Full → Blockbook(REST)**.&#x20;

{% hint style="warning" %}
Do not use the standard Dogecoin JSON-RPC API for address-indexed queries — it has no address index and does not serve `/api/v2/` paths
{% endhint %}
{% endstep %}

{% step %}
Use the access token the configurator returns. Blockbook gets its own endpoint URL, distinct from any Dogecoin JSON-RPC token already on the account, and every path below is appended to that URL
{% endstep %}
{% endstepper %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

### Endpoints

<table data-search="false"><thead><tr><th width="99">Method</th><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/status</code></td><td>Returns the indexer's sync state and the connected node's metadata</td></tr><tr><td>GET</td><td><code>/api/v2/address/{address}</code></td><td>Returns balance and transaction data for a single Dogecoin address</td></tr><tr><td>GET</td><td><code>/api/v2/xpub/{xpub}</code></td><td>Returns wallet-level balance and transaction data for an extended public key or output descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/utxo/{addressOrXpub}</code></td><td>Returns the unspent transaction outputs for an address, extended public key, or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/balancehistory/{address}</code></td><td>Returns aggregated balance-change history over a time range</td></tr><tr><td>GET</td><td><code>/api/v2/tx/{txid}</code></td><td>Returns a normalized transaction by its id</td></tr><tr><td>GET</td><td><code>/api/v2/tx-specific/{txid}</code></td><td>Returns the transaction in the node's own JSON shape</td></tr><tr><td>GET</td><td><code>/api/v2/block/{blockId}</code></td><td>Returns a block by height or hash with a paged list of its transactions</td></tr><tr><td>GET</td><td><code>/api/v2/block-index/{blockHeight}</code></td><td>Returns the block hash at a given block height</td></tr><tr><td>GET</td><td><code>/api/v2/rawblock/{blockId}</code></td><td>Returns the raw serialized hex of a block</td></tr><tr><td>GET</td><td><code>/api/v2/feestats/{blockId}</code></td><td>Returns fee statistics for the transactions in one block</td></tr><tr><td>GET</td><td><code>/api/v2/estimatefee/{blocks}</code></td><td>Returns the backend fee estimate for a confirmation target</td></tr><tr><td>POST</td><td><code>/api/v2/sendtx/</code></td><td>Broadcasts a signed, serialized transaction and returns its transaction id</td></tr><tr><td>GET</td><td><code>/api/v2/tickers/</code></td><td>Returns current or historical fiat exchange rates for Dogecoin</td></tr><tr><td>GET</td><td><code>/api/v2/tickers-list/</code></td><td>Returns the currencies with rate data at a given timestamp</td></tr><tr><td>GET</td><td><code>/api/v2/multi-tickers/</code></td><td>Returns fiat rate tickers for several timestamps at once</td></tr></tbody></table>

### Dogecoin specifics

These differ from the other UTXO chains and are worth reading before integrating.

* **Amounts are in koinu**, at 1e-8 DOGE, and are large in absolute terms. A mid-sized exchange balance runs to sixteen digits. Use a 64-bit integer or a decimal type; a 32-bit integer overflows.
* **A one-block fee target returns `-1`.** The node has no estimate for it, and Blockbook passes the `-1` through. Multiplying it by a transaction size yields a negative fee. Check the estimate is positive, or request 6 blocks or more. See [api/v2/estimatefee](api-v2-estimatefee-dogecoin.md).
* **No SegWit.** Transactions report `size` only, with no `vsize` in the normalized schema, and fee rates are per byte.
* **One-minute blocks.** Heights advance about ten times faster than Bitcoin, so a confirmation depth copied from a Bitcoin integration settles in roughly a tenth of the wall-clock time.
* **Merge-mined with Litecoin.** Block `nonce` is always `0`, and raw blocks carry an AuxPoW header after the standard block header. A parser written for Bitcoin's block format will not read them without handling that.
* **Derivation uses BIP44 coin type 3**, so account paths read `m/44'/3'/0'/...`.

### Wallet RPC methods are not available

| Dogecoin Core wallet method | Blockbook REST equivalent                 |
| --------------------------- | ----------------------------------------- |
| `listunspent`               | `/api/v2/utxo/{address}`                  |
| `listtransactions`          | `/api/v2/address/{address}?details=txs`   |
| `getbalance`                | `/api/v2/address/{address}?details=basic` |
| `getreceivedbyaddress`      | `/api/v2/address/{address}?details=basic` |
| `gettransaction`            | `/api/v2/tx/{txid}`                       |

### Troubleshooting

| Symptom                                         | Cause                                                                           | Fix                                                              |
| ----------------------------------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| A computed fee comes out negative               | `estimatefee` returned `-1` for a one-block target                              | Check the estimate is positive; request 6 blocks or more         |
| `405 Method Not Allowed` on `listunspent`       | Dogecoin Core wallet RPCs are disabled on shared nodes                          | Use `/api/v2/utxo/{address}`                                     |
| An HTML page is returned instead of JSON        | A POST was sent to the endpoint root, which serves the Blockbook web interface  | Send a GET to a `/api/v2/` path                                  |
| `403 Forbidden`                                 | The token is missing, invalid, or belongs to a different endpoint configuration | Confirm the token was provisioned as Dogecoin → Blockbook → REST |
| An address query returns no `address` or `path` | Those fields are returned only for xpub and descriptor queries                  | Expected behaviour; the owning address is the one requested      |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
