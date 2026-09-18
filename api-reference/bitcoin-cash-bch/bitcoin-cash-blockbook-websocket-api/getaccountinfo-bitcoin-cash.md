---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to use
  the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Bitcoin Cash

Returns balance and transaction data for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/address](../bitcoin-cash-blockbook-rest-api/api-v2-address-bitcoin-cash.md) endpoint.

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| descriptor | string | Yes | Address, extended public key, or descriptor to query |
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
        "descriptor": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
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
        "totalPages": 12,
        "itemsOnPage": 1000,
        "address": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
        "balance": "5593730321",
        "totalReceived": "5593730321",
        "totalSent": "0",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 11722,
        "txids": [
            "94dec6ad13dd53825c75e280108669ff5b0658ebbd1acd6081e39e4bf620a235",
            "fde470024107b71206c7e658703d80b3c26c41e8c4782fd2b7d59c325e7723d4",
            "fac3b2c9176fb64d4277438585086b3b71efd0406f5eedf8098b9ab6d12aad25"
        ]
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address or descriptor |
| balance | string | Confirmed balance in satoshis |
| totalReceived | string | Total received in satoshis |
| totalSent | string | Total sent in satoshis |
| unconfirmedBalance | string | Unconfirmed balance in satoshis |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |

{% hint style="info" %}
CashAddr (`bitcoincash:q...`) and legacy Base58 (`1...`) address formats are both accepted. The response echoes the address in the format used in the request.
{% endhint %}

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an account
* **History Pages**: Page through an account's transactions
* **Payment Detection**: Read an address balance after a deposit
* **Reconciliation**: Compare indexed totals against local bookkeeping

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| error | Invalid address | The descriptor is malformed |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
