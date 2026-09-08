---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to
  use the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Dash

Returns an address's balance, transaction count, and history over WebSocket. The descriptor is a Dash address (or xpub); details controls how much history is returned.

## Parameters

| Parameter  | Type   | Required | Description          |
| ---------- | ------ | -------- | -------------------- |
| descriptor | string | Yes      | Dash address or xpub |
| details    | string | Optional | basic, txids, or txs |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getAccountInfo",
    "params": {
        "descriptor": "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN",
        "details": "txids"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "address": "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN",
        "balance": "150000000",
        "totalReceived": "500000000",
        "totalSent": "350000000",
        "txs": 12,
        "txids": [
            "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c"
        ]
    }
}
```

## Response Fields

| Field   | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| balance | string | Confirmed balance in duffs      |
| txs     | number | Transaction count               |
| txids   | array  | Transaction ids for the address |

## Use Cases

* **Wallet Balances**: Read balances over a persistent connection
* **xpub Wallets**: Query a whole wallet by descriptor
* **Dashboards**: Refresh balances without reconnecting

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| error                     | Invalid descriptor | The address or xpub is invalid                    |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
