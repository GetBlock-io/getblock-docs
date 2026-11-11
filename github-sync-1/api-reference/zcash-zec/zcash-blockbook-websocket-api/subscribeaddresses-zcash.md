---
description: >-
  Example code for the subscribeAddresses WebSocket method. Complete guide on how to use
  the subscribeAddresses WebSocket method in the GetBlock Web3 documentation.
---

# subscribeAddresses - Zcash

Subscribes to activity on a set of transparent addresses. When a transaction touches any subscribed address, the server pushes the address and the transaction. This is the basis of payment detection: a deposit is delivered the moment it reaches the mempool.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| addresses | array | Yes | Transparent Zcash addresses to watch (`t1`, `t3`) |
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
            "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
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

While subscribed, the server pushes the address and the full normalized transaction. The message below was captured live; `vin` and `vout` are truncated to one entry each:

```json
{
    "id": "addr",
    "data": {
        "address": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
        "tx": {
            "txid": "542fc3d23d0d8845523f0a962e6aba2626f8e45f64290fbc569e2e80b4248ca4",
            "version": 6,
            "vin": [
                {
                    "txid": "71c66d708dd51510a5d577978881ae5f26e6e869d5a686bd825e78edd3929e9a",
                    "sequence": 4294967295,
                    "n": 0,
                    "addresses": [
                        "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
                    ],
                    "isAddress": true,
                    "value": "15759121998"
                }
            ],
            "vout": [
                {
                    "value": "58256746",
                    "n": 0,
                    "addresses": [
                        "t1dt498tLFo8CqMGRBfMceUTxxK44LFs1HF"
                    ],
                    "isAddress": true
                }
            ],
            "blockHeight": 0,
            "confirmations": 0,
            "confirmationETABlocks": 1,
            "confirmationETASeconds": 81,
            "blockTime": 1789897215,
            "size": 241,
            "value": "15759111998",
            "valueIn": "15759121998",
            "fees": "10000"
        }
    }
}
```

A transaction is pushed as soon as it reaches the mempool, carrying `confirmations: 0`. Confirmation is not delivered as a guaranteed per-transaction follow-up, so do not wait for one to settle a payment: subscribe to [subscribeNewBlock](subscribenewblock-zcash.md) as well and re-read the transaction on each block.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| subscribed | boolean | Confirms the subscription is active |
| address | string | Address that received activity (in notifications) |
| tx | object | The transaction touching the address (in notifications) |
| confirmationETABlocks | integer | Estimated blocks until confirmation, on unconfirmed transactions |
| confirmationETASeconds | integer | Estimated seconds until confirmation, on unconfirmed transactions |

{% hint style="warning" %}
An unconfirmed transaction arrives with **`blockHeight` set to `0`** here, while the REST [api/v2/tx](../zcash-blockbook-rest-api/api-v2-tx-zcash.md) endpoint reports `-1` for the same state. Detect pending status with `confirmations === 0` so one check works across both interfaces.

Notifications reuse the `id` from the subscription request, not a fixed value. Give each subscription on a connection its own `id` so pushes can be routed by `id`.
{% endhint %}

{% hint style="danger" %}
`fees` counts transparent value only. The push above is a transparent transaction, so its `10000` is correct, but a transaction spending from a shielded pool arrives with `vin: []`, `valueIn: "0"`, and `fees: "0"` regardless of the fee actually paid. Do not drive payment logic from `fees` without checking whether the transaction has a shielded component.
{% endhint %}

## Use Cases

* **Payment Detection**: Get notified the instant a transparent deposit reaches the mempool
* **Wallet UX**: Update balances on incoming transactions without polling
* **Confirmation Tracking**: Pair with subscribeNewBlock to re-read depth as blocks arrive
* **Monitoring**: Watch hot wallets in real time

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | One of the addresses is invalid or not transparent |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
