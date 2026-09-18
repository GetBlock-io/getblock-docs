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
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountInfo",
    "params": {
        "descriptor": "XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw",
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
        "page": 1,
        "totalPages": 173,
        "itemsOnPage": 1000,
        "address": "XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw",
        "balance": "219747488042",
        "totalReceived": "9426723694394",
        "totalSent": "9206976206352",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 172603,
        "txids": [
            "f46a956ed9042654fc63fc67a1ac30b4914805659afabe814fa9c2dc65731c40",
            "092fee8b2d026eb7959faa3e3c20dcc37c84b24378abf8904fc775b67b2a197a"
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
