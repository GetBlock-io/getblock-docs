---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to use
  the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Zcash

Returns balance and transaction data for a transparent address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/address](../zcash-blockbook-rest-api/api-v2-address-zcash.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| descriptor | string | Yes | Transparent address (`t1`, `t3`), extended public key, or descriptor |
| details | string | No | Detail level: basic, txids, txslight, or txs. Default txids |
| page | integer | No | 1-based page index for transaction history |
| pageSize | integer | No | History items per page |
| from | integer | No | First block height to include |
| to | integer | No | Last block height to include |
| gap | integer | No | Derivation gap limit for xpub queries |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountInfo",
    "params": {
        "descriptor": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
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
        "totalPages": 21,
        "itemsOnPage": 1000,
        "address": "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua",
        "balance": "14380441165905",
        "totalReceived": "891236821557936",
        "totalSent": "876856380392031",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 20207,
        "txids": [
            "873df9973be48d17d305ec078fdea2c248c9a0e5b592c67b5909fd0a9280cc9f",
            "be70321bbc4539e1cfe648af49ce77e9a8f4d83936a11980336f4b2682e4e431"
        ]
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address or descriptor |
| balance | string | Confirmed transparent balance in zatoshis |
| totalReceived | string | Total received in zatoshis |
| totalSent | string | Total sent in zatoshis |
| unconfirmedBalance | string | Unconfirmed balance in zatoshis |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |

{% hint style="warning" %}
Transparent addresses only. Shielded Sapling and Orchard value is encrypted to the holder's viewing key and is never indexed, so a shielded balance cannot be read here or anywhere else on a public endpoint.
{% endhint %}

## Use Cases

* **Balance Display**: Show the transparent balance of an account
* **History Pages**: Page through an account's transactions
* **Reconciliation**: Compare indexed totals against local bookkeeping

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | The descriptor is malformed or not transparent |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
