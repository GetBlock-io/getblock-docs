---
description: >-
  Example code for the subscribeAddresses WebSocket method. Complete guide on how to use
  the subscribeAddresses WebSocket method in the GetBlock Web3 documentation.
---

# subscribeAddresses - Dogecoin

Subscribes to activity on a set of addresses. When a transaction touches any subscribed address, the server pushes the address and the transaction. This is the basis of payment detection: a deposit is delivered the moment it reaches the mempool.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| addresses | array | Yes | Dogecoin addresses to watch |
| newBlockTxs | boolean | No | When true, also push the subscribed addresses' transactions contained in each new block |

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
            "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF"
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

While subscribed, the server pushes a message carrying the address and the full normalized transaction:

```json
{
    "id": "addr",
    "data": {
        "address": "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF",
        "tx": {
            "txid": "d0dd61c93cd4a571030c15b5f6a94466a8f7246b986dd318d656c910e6e5a8ed",
            "blockHeight": 0,
            "confirmations": 0,
            "confirmationETABlocks": 1,
            "value": "499999999659500",
            "valueIn": "500000000000000",
            "fees": "340500"
        }
    }
}
```

A transaction is pushed as soon as it reaches the mempool, carrying `confirmations: 0`. Confirmation is not delivered as a guaranteed per-transaction follow-up, so do not wait for one to settle a payment: subscribe to [subscribeNewBlock](subscribenewblock-dogecoin.md) as well and re-read the transaction on each block.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| subscribed | boolean | Confirms the subscription is active |
| address | string | Address that received activity (in notifications) |
| tx | object | The transaction touching the address (in notifications) |
| confirmationETABlocks | integer | Estimated blocks until confirmation, on unconfirmed transactions |
| confirmationETASeconds | integer | Estimated seconds until confirmation, on unconfirmed transactions |

{% hint style="warning" %}
An unconfirmed transaction arrives with **`blockHeight` set to `0`** in this notification, while the REST [api/v2/tx](../dogecoin-blockbook-rest-api/api-v2-tx-dogecoin.md) endpoint reports `-1` for the same state. Detect pending status with `confirmations === 0` so one check works across both interfaces.

Notifications reuse the `id` sent with the subscription request, not a fixed value. Give each subscription on a connection its own `id` so pushes can be routed by `id`.
{% endhint %}

## Use Cases

* **Payment Detection**: Get notified the instant a deposit reaches the mempool
* **Wallet UX**: Update balances on incoming transactions without polling
* **Confirmation Tracking**: Pair with subscribeNewBlock to re-read depth as blocks arrive
* **Monitoring**: Watch hot wallets in real time

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | One of the addresses is invalid |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
