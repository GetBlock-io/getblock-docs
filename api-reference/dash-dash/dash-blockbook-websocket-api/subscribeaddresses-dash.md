---
description: >-
  Example code for the subscribeAddresses WebSocket method. Complete guide on
  how to use the subscribeAddresses WebSocket method in the GetBlock Web3
  documentation.
---

# subscribeAddresses - Dash

Subscribes to activity on a set of addresses over WebSocket. When a transaction touches any subscribed address, the server pushes the address and the transaction. Ideal for payment detection.

{% hint style="warning" %}
This is a WebSocket subscription. After the initial acknowledgement, the server pushes notifications until you unsubscribe or disconnect.
{% endhint %}

## Parameters

| Parameter | Type  | Required | Description                      |
| --------- | ----- | -------- | -------------------------------- |
| addresses | array | Yes      | Array of Dash addresses to watch |

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
            "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN"
        ]
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
        "address": "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN",
        "tx": {
            "txid": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c",
            "value": "150000000"
        }
    }
}
```

## Response Fields

| Field      | Type    | Description                                             |
| ---------- | ------- | ------------------------------------------------------- |
| subscribed | boolean | Confirms the subscription is active                     |
| address    | string  | Address that received activity (in notifications)       |
| tx         | object  | The transaction touching the address (in notifications) |

## Use Cases

* **Payment Detection**: Get notified the instant a deposit arrives
* **Wallet UX**: Update balances on incoming transactions
* **Monitoring**: Watch hot wallets in real time

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | One of the addresses is invalid                   |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
