---
description: >-
  GetBlock provides fast and reliable access to Bitcoin nodes via the Blockbook
  WebSocket API. Connect to the Bitcoin network without running your own
  infrastructure.
---

# Bitcoin Blockbook (WebSocket) API

The Blockbook indexer WebSocket interface for Bitcoin: request/response queries (status, account info, UTXOs, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

Blockbook (REST) and Blockbook (WebSocket) are provisioned as separate interfaces with their own endpoint URLs. Enabling one does not enable the other. A project that watches an address as well as querying it needs both.

{% hint style="info" %}
Use WebSocket when subscriptions are required, and [REST](../bitcoin-blockbook-rest-api/) for one-off queries. A standard Bitcoin JSON-RPC endpoint does not support Blockbook methods.
{% endhint %}

{% hint style="warning" %}
Blockbook (WebSocket) is available on **Mainnet only**. Blockbook (REST) is available on both Mainnet and Testnet, so a testnet integration polls REST in place of subscribing.
{% endhint %}

### Base URL

```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket
```

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

### Methods

| Method                                                        | Description                                                     |
| ------------------------------------------------------------- | ---------------------------------------------------------------- |
| [getInfo](getinfo-bitcoin.md)                                 | Indexer and backend status                                      |
| [getAccountInfo](getaccountinfo-bitcoin.md)                   | Address or xpub balance and transaction history                 |
| [getAccountUtxo](getaccountutxo-bitcoin.md)                   | Unspent outputs for an address, xpub, or descriptor             |
| [getTransaction](gettransaction-bitcoin.md)                   | Normalized transaction by txid                                  |
| [sendTransaction](sendtransaction-bitcoin.md)                 | Broadcast a signed, serialized transaction                      |
| [estimateFee](estimatefee-bitcoin.md)                         | Fee estimate for one or more confirmation targets               |
| [subscribeNewBlock](subscribenewblock-bitcoin.md)             | Subscribe to new blocks as they are connected                   |
| [subscribeAddresses](subscribeaddresses-bitcoin.md)           | Subscribe to activity on a set of addresses                     |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
