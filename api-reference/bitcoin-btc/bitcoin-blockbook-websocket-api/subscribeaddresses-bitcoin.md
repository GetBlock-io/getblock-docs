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

| Parameter   | Type    | Required | Description                                                                             |
| ----------- | ------- | -------- | --------------------------------------------------------------------------------------- |
| addresses   | array   | Yes      | Bitcoin addresses to watch                                                              |
| newBlockTxs | boolean | No       | When true, also push the subscribed addresses' transactions contained in each new block |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "addr",
    "method": "subscribeAddresses",
    "params": {
        "addresses": [
            "bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h"
        ],
        "newBlockTxs": true
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "addr",
    "data": {
        "subscribed": true
    }
}
```

## Notifications

While subscribed, the server pushes messages of the form:

```json
{
    "id": "addr",
    "data": {
        "address": "bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h",
        "tx": {
            "txid": "34541782baf07b7523e52881a297f7d44eb6296443c56d9f500cc31f641c5455",
            "version": 1,
            "blockHeight": 0,
            "confirmations": 0,
            "confirmationETABlocks": 1,
            "confirmationETASeconds": 642,
            "blockTime": 1789699761,
            "size": 582,
            "vsize": 501,
            "value": "1430913425",
            "valueIn": "1430915930",
            "fees": "2505"
        }
    }
}
```

A transaction is pushed as soon as it reaches the mempool, carrying `confirmations: 0`. Confirmation is not delivered as a per-transaction follow-up: subscribing with `newBlockTxs: true` adds a push covering the subscribed addresses' transactions in each new block, but a given transaction is not guaranteed a second notification on the block that mines it.

Do not wait for a confirmation push to settle a payment. Subscribe to [subscribeNewBlock](subscribenewblock-bitcoin.md) as well, and on each block re-read the transaction with [getTransaction](gettransaction-bitcoin.md) or the address with [getAccountInfo](getaccountinfo-bitcoin.md) to read its current depth.

## Response Fields

| Field                  | Type    | Description                                                       |
| ---------------------- | ------- | ----------------------------------------------------------------- |
| subscribed             | boolean | Confirms the subscription is active                               |
| address                | string  | Address that received activity (in notifications)                 |
| tx                     | object  | The transaction touching the address (in notifications)           |
| confirmationETABlocks  | integer | Estimated blocks until confirmation, on unconfirmed transactions  |
| confirmationETASeconds | integer | Estimated seconds until confirmation, on unconfirmed transactions |

{% hint style="warning" %}
An unconfirmed transaction arrives with **`blockHeight` set to `0`** in this notification, not `-1`. The REST [api/v2/tx](../bitcoin-blockbook-rest-api/api-v2-tx-bitcoin.md) endpoint reports `-1` for the same state. Detect pending status with `confirmations === 0` rather than by comparing `blockHeight`, so the same check works across both interfaces.
{% endhint %}

{% hint style="info" %}
Notifications reuse the `id` sent with the subscription request, not a fixed value. Give each subscription on a connection its own `id` — `addr` above — so pushes can be routed by `id` without inspecting the payload.
{% endhint %}

## Use Cases

* **Payment Detection**: Get notified the instant a deposit reaches the mempool
* **Confirmation Tracking**: Pair with subscribeNewBlock to re-read depth as blocks arrive
* **Wallet UX**: Update balances on incoming transactions without polling
* **Monitoring**: Watch hot wallets in real time

{% hint style="info" %}
Empty fields are omitted rather than sent as null, so a field's absence is not a signal in itself. `rbf` appears only when the transaction signals replace-by-fee, meaning it can still be replaced while unconfirmed; treat such a deposit as pending until it reaches the confirmation depth the payment flow requires. `confirmationETASeconds` is likewise present only while the transaction is unconfirmed.
{% endhint %}

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | One of the addresses is invalid                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
