---
description: >-
  GetBlock provides fast and reliable access to Zcash transparent address, UTXO,
  and wallet data via the Blockbook REST API. Connect to the Zcash network
  without running your own infrastructure.
---

# Zcash Blockbook REST API

Blockbook is an address-indexed and xpub-indexed view of the Zcash chain, built by Trezor. It answers questions about a transparent address or a transparent wallet: balances, transaction history, and unspent outputs.

GetBlock's Blockbook add-on is provisioned as its own endpoint, separate from the Zcash JSON-RPC endpoint and with its own URL.

{% hint style="warning" %}
**Blockbook sees transparent activity only.** Shielded Sapling and Orchard value is encrypted to the holder's viewing key, so it is never indexed. Two consequences follow:

* Balances, UTXOs, and history cover transparent (`t1`, `t3`) addresses and nothing else.
* In the normalized transaction schema, **a shielded transaction's `fees` usually reads `0`**, because the value entering from a shielded pool is invisible. See [api/v2/tx](api-v2-tx-zcash.md) for a worked example and how to compute the real fee.
{% endhint %}

### Setting up a Blockbook endpoint

{% stepper %}
{% step %}
Sign in to [GetBlock](https://account.getblock.io) and start a new endpoint in the configurator
{% endstep %}

{% step %}
Select **Zcash → Mainnet → Full → Blockbook(REST)**.

{% hint style="warning" %}
Do not use the Zcash JSON-RPC endpoint for these paths. It does not serve `/api/v2/`.
{% endhint %}
{% endstep %}

{% step %}
Use the access token the configurator returns. Every path below is appended to that URL
{% endstep %}
{% endstepper %}

### Base URL

```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/
```

### Endpoints

<table data-search="false"><thead><tr><th width="99">Method</th><th>Endpoint</th><th>Description</th></tr></thead><tbody><tr><td>GET</td><td><code>/api/status</code></td><td>Returns the indexer's sync state, the node's metadata, and the consensus branch ID</td></tr><tr><td>GET</td><td><code>/api/v2/address/{address}</code></td><td>Returns balance and transaction data for a transparent address</td></tr><tr><td>GET</td><td><code>/api/v2/xpub/{xpub}</code></td><td>Returns wallet-level data for a transparent extended public key or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/utxo/{addressOrXpub}</code></td><td>Returns the unspent transparent outputs for an address, extended public key, or descriptor</td></tr><tr><td>GET</td><td><code>/api/v2/balancehistory/{address}</code></td><td>Returns aggregated balance-change history over a time range</td></tr><tr><td>GET</td><td><code>/api/v2/tx/{txid}</code></td><td>Returns a normalized transaction, counting transparent value only</td></tr><tr><td>GET</td><td><code>/api/v2/tx-specific/{txid}</code></td><td>Returns the transaction in the node's own shape, including shielded components</td></tr><tr><td>GET</td><td><code>/api/v2/block/{blockId}</code></td><td>Returns a block by height or hash with a paged list of its transactions</td></tr><tr><td>GET</td><td><code>/api/v2/block-index/{blockHeight}</code></td><td>Returns the block hash at a given block height</td></tr><tr><td>GET</td><td><code>/api/v2/feestats/{blockId}</code></td><td>Returns fee statistics for one block, from transparent value only</td></tr><tr><td>POST</td><td><code>/api/v2/sendtx/</code></td><td>Broadcasts a signed, serialized transaction and returns its transaction id</td></tr><tr><td>GET</td><td><code>/api/v2/tickers/</code></td><td>Returns current or historical fiat exchange rates for Zcash</td></tr><tr><td>GET</td><td><code>/api/v2/tickers-list/</code></td><td>Returns the currencies with rate data at a given timestamp</td></tr><tr><td>GET</td><td><code>/api/v2/multi-tickers/</code></td><td>Returns fiat rate tickers for several timestamps at once</td></tr></tbody></table>

### Not available on Zcash

Two Blockbook endpoints offered on the other UTXO chains return `500 Internal server error` on Zcash and are not documented here:

| Endpoint | Why it fails | Use instead |
| --- | --- | --- |
| `/api/v2/estimatefee/{blocks}` | Zebra implements neither `estimatesmartfee` nor `estimatefee`, so the indexer has no estimate to return | The ZIP-317 conventional fee: 5,000 zatoshis per logical action, with a minimum of two actions |
| `/api/v2/rawblock/{blockId}` | Fails for every block tested | JSON-RPC [getblock](../zcash-json-rpc-api/getblock-zcash.md) with verbosity `0` |

### Zcash specifics

* **Amounts are in zatoshis**, at 1e-8 ZEC.
* **Fees follow ZIP-317**, not a fee market: 5,000 zatoshis per logical action, minimum two actions.
* **75-second blocks.** Heights advance about eight times as fast as Bitcoin's.
* **Consensus branch IDs.** Transactions are signed for the network upgrade in force, reported under `backend.consensus` by [api/status](api-status-zcash.md).
* **Transaction expiry.** Zcash transactions carry an `expiryheight` and cannot be mined after it.
* **Derivation uses BIP44 coin type 133** for transparent accounts.

### Troubleshooting

| Symptom | Cause | Fix |
| --- | --- | --- |
| A shielded transaction shows `fees: "0"` | The normalized schema cannot see shielded value | Compute the fee from [api/v2/tx-specific](api-v2-tx-specific-zcash.md) value balances |
| `500 Internal server error` on `estimatefee` | The Zebra node has no fee estimator | Use the ZIP-317 conventional fee |
| `Invalid address, decoded address is of unknown format` | A shielded or malformed address was supplied | Query a transparent (`t1`, `t3`) address |
| `403 Forbidden` | The token is missing, invalid, or belongs to a different endpoint configuration | Confirm the token was provisioned as Zcash → Blockbook(REST) |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
