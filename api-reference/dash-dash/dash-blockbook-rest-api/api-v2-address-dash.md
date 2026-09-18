---
description: >-
  Example code for the api/v2/address REST method. Complete guide on how to use
  the api/v2/address REST method in the GetBlock Web3 documentation.
---

# api/v2/address - Dash

This endpoint returns balance and transaction data for a single Dash address. The details query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type    | Location | Required | Description                                                 |
| --------- | ------- | -------- | -------- | ----------------------------------------------------------- |
| address   | string  | path     | Yes      | The Dash address to query                           |
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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw?page=1&pageSize=1000&details=txids')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "page": 1,
    "totalPages": 173,
    "itemsOnPage": 1000,
    "address": "XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw",
    "balance": "219706393531",
    "totalReceived": "9426682599883",
    "totalSent": "9206976206352",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 172602,
    "txids": [
        "092fee8b2d026eb7959faa3e3c20dcc37c84b24378abf8904fc775b67b2a197a",
        "28fd12e061c1e086e9861cfd396e6bc1afcdfc28c245cb487e4fe421cefbd926",
        "200644c0a57556437ce3aa9b30fc9ac803496c3f182477f8886ffa00c6efdf89"
    ]
}
```

## Response Parameters

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| address            | string  | The queried address                                   |
| balance            | string  | Confirmed balance in duffs                         |
| totalReceived      | string  | Total received in duffs                            |
| totalSent          | string  | Total sent in duffs                                |
| unconfirmedBalance | string  | Unconfirmed balance in duffs                       |
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
| 400         | Bad request    | The address is malformed or not a valid Dash address |
| 404         | Not found      | No indexed data exists for the requested address             |
| 500         | Internal error | The indexer failed to read address data                      |
