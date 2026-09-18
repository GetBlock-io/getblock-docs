---
description: >-
  Example code for the subscribeAddresses WebSocket method. Complete guide on
  how to use the subscribeAddresses WebSocket method in the GetBlock Web3
  documentation.
---

# subscribeAddresses - Bitcoin

Subscribes to activity on a set of addresses over WebSocket. When a transaction touches any subscribed address, the server pushes the address and the transaction. This is the basis of payment detection: a deposit is delivered the moment it reaches the mempool, and again when it is mined.

{% hint style="warning" %}
This is a WebSocket subscription. After the initial acknowledgement, the server pushes notifications until the connection unsubscribes or disconnects. One subscription replaces the previous one on the same connection, so send the complete address set in a single call rather than one call per address.
{% endhint %}

## Parameters

| Parameter   | Type    | Required | Description                                                                        |
| ----------- | ------- | -------- | ----------------------------------------------------------------------------------- |
| addresses   | array   | Yes      | Bitcoin addresses to watch                                                          |
| newBlockTxs | boolean | No       | When true, also push the subscribed addresses' transactions contained in each new block |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "subscribeAddresses",
    "params": {
        "addresses": [
            "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
        ],
        "newBlockTxs": true
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "subscribed": true
    }
}
```

## Notifications

While subscribed, the server pushes messages of the form:

```json
{
    "id": "getblock.io",
    "data": {
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "tx": {
            "txid": "8c1e3dec662d1f2a5e322ccef5eca263f98eb16723c6f990be0c88c1db113fb1",
            "blockHeight": -1,
            "confirmations": 0,
            "blockTime": 1725959035,
            "value": "10063100",
            "valueIn": "10106300",
            "fees": "43200",
            "rbf": true
        }
    }
}
```

A transaction is pushed twice over its lifetime: once on arrival in the mempool, with `blockHeight` set to `-1` and `confirmations` set to `0`, and again once mined, with the real block height and `confirmations` of `1`. Deeper confirmations are not pushed per address; track them with [subscribeNewBlock](subscribenewblock-bitcoin.md) and compare against the transaction's block height.

## Response Fields

| Field      | Type    | Description                                             |
| ---------- | ------- | ------------------------------------------------------- |
| subscribed | boolean | Confirms the subscription is active                     |
| address    | string  | Address that received activity (in notifications)       |
| tx         | object  | The transaction touching the address (in notifications) |

## Use Cases

* **Payment Detection**: Get notified the instant a deposit reaches the mempool
* **Confirmation Tracking**: Detect the transition from pending to first confirmation
* **Wallet UX**: Update balances on incoming transactions without polling
* **Monitoring**: Watch hot wallets in real time

{% hint style="info" %}
`rbf: true` marks a transaction that signals replace-by-fee and can still be replaced while unconfirmed. Treat such a deposit as pending until it reaches the confirmation depth the payment flow requires.
{% endhint %}

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | One of the addresses is invalid                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
