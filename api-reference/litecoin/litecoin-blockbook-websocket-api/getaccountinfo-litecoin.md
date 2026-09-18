---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to use
  the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Litecoin

Returns balance and transaction data for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/address](../litecoin-blockbook-rest-api/api-v2-address-litecoin.md) and [api/v2/xpub](../litecoin-blockbook-rest-api/api-v2-xpub-litecoin.md) endpoints.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| descriptor | string | Yes | Address, extended public key, or descriptor to query |
| details | string | No | Detail level: basic, txids, txslight, or txs. Default txids |
| tokens | string | No | Which xpub-derived addresses to include: nonzero, used, or derived |
| page | integer | No | 1-based page index for transaction history |
| pageSize | integer | No | History items per page |
| from | integer | No | First block height to include |
| to | integer | No | Last block height to include |
| gap | integer | No | Derivation gap limit for xpub queries |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getAccountInfo",
    "params": {
        "descriptor": "LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5",
        "details": "txids",
        "page": 1,
        "pageSize": 1000
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
        "totalPages": 1,
        "itemsOnPage": 1000,
        "address": "LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5",
        "balance": "111286503113",
        "totalReceived": "350564914665",
        "totalSent": "239278411552",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 152,
        "txids": [
            "51a31c4236a99bfd17cda606f079c9c4814851119e7593ab7c739bd5972b1969",
            "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7"
        ]
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address or descriptor |
| balance | string | Confirmed balance in litoshis |
| totalReceived | string | Total received in litoshis |
| totalSent | string | Total sent in litoshis |
| unconfirmedBalance | string | Unconfirmed balance in litoshis |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an account
* **History Pages**: Page through an account's transactions
* **Wallet Loading**: Read a whole xpub wallet in one call
* **Reconciliation**: Compare indexed totals against local bookkeeping

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | The descriptor is malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
