---
description: >-
  GetBlock provides fast and reliable access to Zcash nodes via the Blockbook
  WebSocket API. Connect to the Zcash network without running your own
  infrastructure.
---

# Zcash Blockbook (WebSocket) API

The Blockbook indexer WebSocket interface for Zcash: request/response queries (status, account info, transparent UTXOs, transactions, broadcast) plus subscriptions to new blocks and address activity over a persistent connection.

Blockbook (REST) and Blockbook (WebSocket) are provisioned as separate interfaces with their own endpoint URLs. Enabling one does not enable the other.

{% hint style="warning" %}
**Blockbook sees transparent activity only.** Shielded Sapling and Orchard value is encrypted to the holder's viewing key and is never indexed. In the normalized schema a shielded transaction arrives with `vin: []`, `valueIn: "0"`, and **`fees: "0"`**, whatever it actually paid. No WebSocket method exposes shielded components; read those from REST [api/v2/tx-specific](../zcash-blockbook-rest-api/api-v2-tx-specific-zcash.md) or JSON-RPC [getrawtransaction](../zcash-json-rpc-api/getrawtransaction-zcash.md).
{% endhint %}

### Base URL

```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>
```

{% hint style="warning" %}
The WebSocket endpoint is the access-token root. Do **not** append `/websocket`: that path returns `404 Not Found` and the upgrade handshake fails before a connection is opened.
{% endhint %}

### Message Envelope

Every request carries an `id` chosen by the client, a `method`, and a `params` object. The server echoes the same `id` on the matching response, so replies can be correlated on a multiplexed connection.

```json
{
    "id": "getblock.io",
    "method": "getInfo",
    "params": {}
}
```

A failed request returns an `error` object in place of the result:

```json
{
    "id": "getblock.io",
    "data": {
        "error": {
            "message": "Invalid address"
        }
    }
}
```

{% hint style="info" %}
Subscription notifications reuse the `id` from the subscription request rather than a fixed value. Give each subscription on a connection its own `id` so pushes can be routed by `id` without inspecting the payload.
{% endhint %}

### Methods

| Method                                                 | Description                                           |
| ------------------------------------------------------ | ------------------------------------------------------- |
| [getInfo](getinfo-zcash.md)                            | Indexer status and the backend's consensus branch IDs |
| [getAccountInfo](getaccountinfo-zcash.md)              | Transparent address balance and transaction history   |
| [getAccountUtxo](getaccountutxo-zcash.md)              | Unspent transparent outputs for an address or xpub    |
| [getTransaction](gettransaction-zcash.md)              | Normalized transaction by txid                        |
| [sendTransaction](sendtransaction-zcash.md)            | Broadcast a signed, serialized transaction            |
| [subscribeNewBlock](subscribenewblock-zcash.md)        | Subscribe to new blocks as they are connected         |
| [subscribeAddresses](subscribeaddresses-zcash.md)      | Subscribe to activity on a set of addresses           |

### Not available on Zcash

| Method | Behaviour | Use instead |
| --- | --- | --- |
| `estimateFee` | Returns `-32601: Method not found`. Zebra implements neither `estimatesmartfee` nor `estimatefee`, so the indexer has nothing to call | The ZIP-317 conventional fee: 5,000 zatoshis per logical action, with a minimum of two actions |

### Zcash specifics

* **Amounts are in zatoshis**, at 1e-8 ZEC.
* **Fees follow ZIP-317**, not a fee market, and there is no fee estimator on any interface.
* **75-second blocks**, so block notifications arrive about eight times as often as on Bitcoin.
* **Consensus branch IDs** are reported by [getInfo](getinfo-zcash.md) under `backend.consensus`; transactions must be signed for the branch in force.
* **Transaction expiry.** Zcash transactions carry an `expiryheight` and cannot be mined after it, so a stuck transaction must be rebuilt rather than rebroadcast.
* **Transaction versions** 5 and 6 are both current; version 6 accompanies the NU6 upgrades.

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
