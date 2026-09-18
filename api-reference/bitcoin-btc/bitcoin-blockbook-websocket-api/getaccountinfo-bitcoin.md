---
description: >-
  Example code for the getAccountInfo WebSocket method. Complete guide on how to
  use the getAccountInfo WebSocket method in the GetBlock Web3 documentation.
---

# getAccountInfo - Bitcoin

Returns balance and transaction data for an address, extended public key, or descriptor over WebSocket. The `details` parameter controls how much data is returned, from a balance-only summary to full transaction objects. This is the WebSocket form of the REST [api/v2/address](../bitcoin-blockbook-rest-api/api-v2-address-bitcoin.md) and [api/v2/xpub](../bitcoin-blockbook-rest-api/api-v2-xpub-bitcoin.md) endpoints.

## Parameters

| Parameter  | Type    | Required | Description                                                        |
| ---------- | ------- | -------- | ------------------------------------------------------------------ |
| descriptor | string  | Yes      | Address, extended public key, or descriptor to query               |
| details    | string  | No       | Detail level: basic, txids, txslight, or txs. Default txids        |
| tokens     | string  | No       | Which xpub-derived addresses to include: nonzero, used, or derived |
| page       | integer | No       | 1-based page index for transaction history                         |
| pageSize   | integer | No       | History items per page                                             |
| from       | integer | No       | First block height to include when filtering history               |
| to         | integer | No       | Last block height to include when filtering history                |
| gap        | integer | No       | Derivation gap limit for xpub queries, capped by the server        |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getAccountInfo",
    "params": {
        "descriptor": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
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
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "balance": "17938214",
        "totalReceived": "17952507",
        "totalSent": "14293",
        "unconfirmedBalance": "0",
        "unconfirmedTxs": 0,
        "txs": 116,
        "txids": [
            "1d51c696eab35d7dad7631516cc0845aaab317fe67cb182782752547541d30ea",
            "a6494142e2e565b5e672d41a37a3eafec2fe5594f22efbb07f421a6cedf473c5"
        ]
    }
}
```

## Response Fields

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| address            | string  | The queried address or descriptor                     |
| balance            | string  | Confirmed balance in satoshis                         |
| totalReceived      | string  | Total received in satoshis                            |
| totalSent          | string  | Total sent in satoshis                                |
| unconfirmedBalance | string  | Unconfirmed balance in satoshis                       |
| unconfirmedTxs     | integer | Number of unconfirmed transactions                    |
| txs                | integer | Number of confirmed transactions                      |
| txids              | array   | Transaction ids, present when details is txids        |
| transactions       | array   | Full transaction objects, present when details is txs |

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an account
* **History Pages**: Page through an account's transaction ids or full transactions
* **Wallet Loading**: Read a whole xpub wallet in one call
* **Reconciliation**: Compare indexed totals against local bookkeeping

{% hint style="info" %}
At `details=basic` the mempool figures are not aggregated: `unconfirmedBalance` is omitted and `unconfirmedTxs` reports the raw mempool index size for the account. Use `txids` or higher when pending amounts matter.
{% endhint %}

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| error                     | Invalid address | The descriptor is malformed                       |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
