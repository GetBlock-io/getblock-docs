---
description: >-
  Example code for the api/v2/address REST method. Complete guide on how to use the
  api/v2/address REST method in the GetBlock Web3 documentation.
---

# api/v2/address - Litecoin

This endpoint returns balance and transaction data for a single Litecoin address. The `details` query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | The Litecoin address to query. Legacy, P2SH, and bech32 (`ltc1`) formats are all accepted |
| page | integer | query | No | 1-based page index for transaction history. Default 1 |
| pageSize | integer | query | No | History items per page. Default and maximum is 1000 |
| from | integer | query | No | First block height to include when filtering history |
| to | integer | query | No | Last block height to include when filtering history |
| details | string | query | No | Detail level: basic, txids, txslight, or txs. Default txids |
| secondary | string | query | No | Secondary fiat currency code for converted values |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?page=1&pageSize=1000&details=txids')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
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
        "4c3d9548bb03a42551c10f2472e99cd86776ed00fd3bfb530c4466b93e7ddac7",
        "edf101ff2a75f99bedafd3ddb046a47b33aa1f0e714d1f4e884e5eb84cd661a3"
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address |
| balance | string | Confirmed balance in litoshis |
| totalReceived | string | Total received in litoshis |
| totalSent | string | Total sent in litoshis |
| unconfirmedBalance | string | Unconfirmed balance in litoshis |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |
| transactions | array | Full transaction objects, present when details is txs |

{% hint style="info" %}
At `details=basic` the mempool figures are not aggregated: `unconfirmedBalance` is omitted and `unconfirmedTxs` reports the raw mempool index size. Use `txids` or higher when pending amounts matter.
{% endhint %}

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an address
* **History Pages**: Page through an address's transaction ids or full transactions
* **Payment Detection**: Detect incoming payments by watching an address balance
* **Accounting**: Read total received and sent for bookkeeping

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The address is malformed or not a valid Litecoin address |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the requested address |
| 500 | Internal error | The indexer failed to read address data |
