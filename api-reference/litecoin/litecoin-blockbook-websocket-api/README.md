---
description: >-
  GetBlock provides fast and reliable access to Litecoin nodes via the Blockbook
  WebSocket API. Connect to the Litecoin network without running your own
  infrastructure.
---

# Litecoin Blockbook (WebSocket) API

The Blockbook indexer WebSocket interface for Litecoin: request/response queries (status, account info, UTXOs, transactions, broadcast, fee estimation) plus subscriptions to new blocks and address activity over a persistent connection.

Blockbook (REST) and Blockbook (WebSocket) are provisioned as separate interfaces with their own endpoint URLs. Enabling one does not enable the other. A project that watches an address and queries it needs both.

{% hint style="info" %}
Use WebSocket when subscriptions are required, and [REST](../litecoin-blockbook-rest-api/) for one-off queries. A standard [Litecoin JSON-RPC](../litecoin-json-rpc-api/) endpoint does not support Blockbook methods, and the Litecoin Core wallet RPCs such as `listunspent` are disabled on shared nodes.
{% endhint %}

### Base URL

```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>
```

### Methods

| Method                                               | Description                                         |
| ---------------------------------------------------- | --------------------------------------------------- |
| [getInfo](getinfo-litecoin.md)                       | Indexer and backend status                          |
| [getAccountInfo](getaccountinfo-litecoin.md)         | Address or xpub balance and transaction history     |
| [getAccountUtxo](getaccountutxo-litecoin.md)         | Unspent outputs for an address, xpub, or descriptor |
| [getTransaction](gettransaction-litecoin.md)         | Normalized transaction by txid                      |
| [sendTransaction](sendtransaction-litecoin.md)       | Broadcast a signed, serialized transaction          |
| [estimateFee](estimatefee-litecoin.md)               | Fee estimate for one or more confirmation targets   |
| [subscribeNewBlock](subscribenewblock-litecoin.md)   | Subscribe to new blocks as they are connected       |
| [subscribeAddresses](subscribeaddresses-litecoin.md) | Subscribe to activity on a set of addresses         |

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
