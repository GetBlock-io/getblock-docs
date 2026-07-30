---
description: >-
  Example code for the api/v2/address REST method. Complete guide on how to use
  the api/v2/address REST method in the GetBlock Web3 documentation.
---

# api/v2/address - Bitcoin Cash

This endpoint returns balance and transaction data for a single Bitcoin Cash address. The details query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type    | Location | Required | Description                                                 |
| --------- | ------- | -------- | -------- | ----------------------------------------------------------- |
| address   | string  | path     | Yes      | The Bitcoin Cash address to query                           |
| page      | integer | query    | No       | 1-based page index for transaction history. Default 1       |
| pageSize  | integer | query    | No       | History items per page. Default and maximum is 1000         |
| from      | integer | query    | No       | First block height to include when filtering history        |
| to        | integer | query    | No       | Last block height to include when filtering history         |
| details   | string  | query    | No       | Detail level: basic, txids, txslight, or txs. Default txids |
| filter    | string  | query    | No       | Filter history by input or output side of the address       |
| secondary | string  | query    | No       | Secondary fiat currency code for converted values           |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/address/bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a?page=1&pageSize=1000&details=txids')

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
    "address": "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a",
    "balance": "9407625",
    "totalReceived": "45231890",
    "totalSent": "35824265",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 5,
    "txids": [
        "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
        "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587"
    ]
}
```

## Response Parameters

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| address            | string  | The queried address                                   |
| balance            | string  | Confirmed balance in satoshis                         |
| totalReceived      | string  | Total received in satoshis                            |
| totalSent          | string  | Total sent in satoshis                                |
| unconfirmedBalance | string  | Unconfirmed balance in satoshis                       |
| txs                | integer | Number of confirmed transactions                      |
| txids              | array   | Transaction ids, present when details is txids        |
| transactions       | array   | Full transaction objects, present when details is txs |

## Use Cases

* **Balance Display**: Show the confirmed and unconfirmed balance of an address
* **History Pages**: Page through an address's transaction ids or full transactions
* **Payment Detection**: Detect incoming payments by watching an address balance
* **Accounting**: Read total received and sent for bookkeeping
* **Block Filtering**: Restrict history to a block-height range with from and to

## Error Handling

| HTTP Status | Message        | Description                                                  |
| ----------- | -------------- | ------------------------------------------------------------ |
| 400         | Bad request    | The address is malformed or not a valid Bitcoin Cash address |
| 404         | Not found      | No indexed data exists for the requested address             |
| 500         | Internal error | The indexer failed to read address data                      |
