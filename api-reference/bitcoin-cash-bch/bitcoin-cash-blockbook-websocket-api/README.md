---
description: >-
  GetBlock provides fast and reliable access to Bitcoin Cash nodes via the
  Blockbook WebSocket API. Connect to the Bitcoin Cash network without running
  your own infrastructure.
---

# Bitcoin Cash Blockbook (WebSocket) API

The Blockbook indexer WebSocket interface for Bitcoin Cash: request/response queries (status, account info, UTXOs, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

Blockbook (REST) and Blockbook (WebSocket) are provisioned as separate interfaces with their own endpoint URLs. Enabling one does not enable the other. A project that watches an address as well as querying it needs both.

{% hint style="info" %}
Use WebSocket when subscriptions are required, and [REST](../bitcoin-cash-blockbook-rest-api/) for one-off queries. A standard Bitcoin Cash JSON-RPC endpoint does not support Blockbook methods, and the Bitcoin Cash Node wallet RPCs such as `listunspent` are disabled on shared nodes.
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

| Method                                                       | Description                                         |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| [getInfo](getinfo-bitcoin-cash.md)                           | Indexer and backend status                          |
| [getAccountInfo](getaccountinfo-bitcoin-cash.md)             | Address balance and transaction history             |
| [getAccountUtxo](getaccountutxo-bitcoin-cash.md)             | Unspent outputs for an address, xpub, or descriptor |
| [getTransaction](gettransaction-bitcoin-cash.md)             | Normalized transaction by txid                      |
| [sendTransaction](sendtransaction-bitcoin-cash.md)           | Broadcast a signed, serialized transaction          |
| [estimateFee](estimatefee-bitcoin-cash.md)                   | Fee estimate for one or more confirmation targets   |
| [subscribeNewBlock](subscribenewblock-bitcoin-cash.md)       | Subscribe to new blocks as they are connected       |
| [subscribeAddresses](subscribeaddresses-bitcoin-cash.md)     | Subscribe to activity on a set of addresses         |

### Bitcoin Cash specifics

* **No SegWit.** Transactions carry no `vsize` or witness data; only `size` is reported, and fee rates are per byte rather than per virtual byte.
* **Address formats.** CashAddr (`bitcoincash:q...`) and legacy Base58 (`1...`) are both accepted wherever an address is taken.
* **Coin name.** `getInfo` reports `name` as `Bcash`, the indexer's internal coin name, and omits the `network` field that the other Blockbook chains return.
* **Shared genesis.** `block0Hash` is the Bitcoin genesis block, since Bitcoin Cash shares history with Bitcoin up to the 2017 fork, so it cannot be used alone to tell the two chains apart.

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
