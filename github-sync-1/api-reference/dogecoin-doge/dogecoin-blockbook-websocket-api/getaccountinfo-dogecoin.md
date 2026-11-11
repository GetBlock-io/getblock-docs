---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to use
  the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Dogecoin

Returns balance and transaction data for an address, extended public key, or descriptor over WebSocket. This is the WebSocket form of the REST [api/v2/address](../dogecoin-blockbook-rest-api/api-v2-address-dogecoin.md) endpoint.

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
        "descriptor": "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF",
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
        "totalPages": 27,
        "itemsOnPage": 1000,
        "address": "DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF",
        "balance": "1105234012375840",
        "totalReceived": "1631025972011211333",
        "totalSent": "1629920737998835493",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 26270,
        "txids": [
            "4c5519699240ed1f23e1ec46ea4bc85f019982f6ef48afdb7698054230a4b76e",
            "8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f"
        ]
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address or descriptor |
| balance | string | Confirmed balance in koinu |
| totalReceived | string | Total received in koinu |
| totalSent | string | Total sent in koinu |
| unconfirmedBalance | string | Unconfirmed balance in koinu |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |

{% hint style="info" %}
Amounts are in koinu at 1e-8 DOGE and run to sixteen digits on active accounts. Parse them with a 64-bit integer or a decimal type; a 32-bit integer or a JavaScript `Number` used carelessly will lose precision.
{% endhint %}

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
