---
description: >-
  GetBlock provides fast and reliable access to Dogecoin nodes via the Blockbook
  WebSocket API. Connect to the Dogecoin network without running your own
  infrastructure.
---

# Dogecoin Blockbook (WebSocket) API

The Blockbook indexer WebSocket interface for Dogecoin: request/response queries (status, account info, UTXOs, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

Blockbook (REST) and Blockbook (WebSocket) are provisioned as separate interfaces with their own endpoint URLs. Enabling one does not enable the other. A project that watches an address as well as querying it needs both.

{% hint style="info" %}
Use WebSocket when subscriptions are required, and [REST](../dogecoin-blockbook-rest-api/) for one-off queries. A standard Dogecoin JSON-RPC endpoint does not support Blockbook methods, and the Dogecoin Core wallet RPCs such as `listunspent` are disabled on shared nodes.
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

| Method                                                    | Description                                         |
| ----------------------------------------------------------- | ----------------------------------------------------- |
| [getInfo](getinfo-dogecoin.md)                            | Indexer and backend status                          |
| [getAccountInfo](getaccountinfo-dogecoin.md)              | Address balance and transaction history             |
| [getAccountUtxo](getaccountutxo-dogecoin.md)              | Unspent outputs for an address, xpub, or descriptor |
| [getTransaction](gettransaction-dogecoin.md)              | Normalized transaction by txid                      |
| [sendTransaction](sendtransaction-dogecoin.md)            | Broadcast a signed, serialized transaction          |
| [estimateFee](estimatefee-dogecoin.md)                    | Fee estimate for one or more confirmation targets   |
| [subscribeNewBlock](subscribenewblock-dogecoin.md)        | Subscribe to new blocks as they are connected       |
| [subscribeAddresses](subscribeaddresses-dogecoin.md)      | Subscribe to activity on a set of addresses         |

### Dogecoin specifics

* **A one-block fee target returns negative values.** `estimateFee` reports `feePerUnit: "-100000000"` for `blocks: 1`, the node's `-1` scaled into koinu. Check the rate is positive before using it. See [estimateFee](estimatefee-dogecoin.md).
* **Amounts are in koinu**, at 1e-8 DOGE, and run to sixteen digits on active accounts. Use a 64-bit integer or a decimal type.
* **No SegWit.** Transactions report `size` only, with no `vsize`, and fee rates are per byte.
* **One-minute blocks.** Block notifications arrive about ten times as often as on Bitcoin, and a confirmation depth copied from a Bitcoin integration settles in roughly a tenth of the wall-clock time.

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
