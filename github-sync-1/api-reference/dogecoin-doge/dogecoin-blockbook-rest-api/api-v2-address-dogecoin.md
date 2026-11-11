---
description: >-
  Example code for the api/v2/address REST method. Complete guide on how to use the
  api/v2/address REST method in the GetBlock Web3 documentation.
---

# api/v2/address - Dogecoin

This endpoint returns balance and transaction data for a single Dogecoin address. The `details` query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | The Dogecoin address to query |
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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?page=1&pageSize=1000&details=txids')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
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
        "8d99a37f27d702b35bcc74dfea0d1a006a67c20571e1de660a30defc16316c8f",
        "c24352909b6f21aff2d499add3858a6d3521a56a30fdba1e085a92ecb85c9205"
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| address | string | The queried address |
| balance | string | Confirmed balance in koinu |
| totalReceived | string | Total received in koinu |
| totalSent | string | Total sent in koinu |
| unconfirmedBalance | string | Unconfirmed balance in koinu |
| unconfirmedTxs | integer | Number of unconfirmed transactions |
| txs | integer | Number of confirmed transactions |
| txids | array | Transaction ids, present when details is txids |
| transactions | array | Full transaction objects, present when details is txs |

{% hint style="info" %}
Amounts are in koinu, the Dogecoin base unit, at 1e-8 DOGE. Values are large in absolute terms: the balance above is roughly 11.05 million DOGE. Use an integer or decimal type wide enough to hold them; a 32-bit integer will overflow.
{% endhint %}

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an address
* **History Pages**: Page through an address's transaction ids or full transactions
* **Payment Detection**: Detect incoming payments by watching an address balance
* **Accounting**: Read total received and sent for bookkeeping

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
