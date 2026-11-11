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
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>
```

### Quickstart

Every request carries an `id` chosen by the client, a `method`, and a `params` object. The server echoes the same `id` on the matching response, so replies can be correlated on a multiplexed connection.

{% tabs %}
{% tab title="request" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getInfo",
    "params": {}
}
```
{% endtab %}

{% tab title="successful response" %}
{% code overflow="wrap" %}
```bash
{
    "id": "getblock.io",
    "data": {
        "name": "Bitcoin",
        "shortcut": "BTC",
        "network": "BTC",
        "decimals": 8,
        "version": "unknown",
        "bestHeight": 967494,
        "bestHash": "00000000000000000000890e285e32408f9da4f2d58b620b5703839448b9c70c",
        "block0Hash": "000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f",
        "testnet": false,
        "backend": {
            "version": "310100",
            "subversion": "/Satoshi:31.1.0/"
        }
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><a href="getinfo-bitcoin.md">getInfo</a></td><td>Indexer and backend status</td></tr><tr><td><a href="getaccountinfo-bitcoin.md">getAccountInfo</a></td><td>Address or xpub balance and transaction history</td></tr><tr><td><a href="getaccountutxo-bitcoin.md">getAccountUtxo</a></td><td>Unspent outputs for an address, xpub, or descriptor</td></tr><tr><td><a href="gettransaction-bitcoin.md">getTransaction</a></td><td>Normalized transaction by txid</td></tr><tr><td><a href="sendtransaction-bitcoin.md">sendTransaction</a></td><td>Broadcast a signed, serialized transaction</td></tr><tr><td><a href="estimatefee-bitcoin.md">estimateFee</a></td><td>Fee estimate for one or more confirmation targets</td></tr><tr><td><a href="subscribenewblock-bitcoin.md">subscribeNewBlock</a></td><td>Subscribe to new blocks as they are connected</td></tr><tr><td><a href="subscribeaddresses-bitcoin.md">subscribeAddresses</a></td><td>Subscribe to activity on a set of addresses</td></tr></tbody></table>

### Support

* Support: [support@getblock.io](mailto:support@getblock.io)
