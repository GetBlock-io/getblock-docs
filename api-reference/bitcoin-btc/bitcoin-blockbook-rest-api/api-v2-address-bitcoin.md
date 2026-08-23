# api v2 address - Bitcoin

This endpoint returns balance and transaction data for a single Bitcoin address. The details query parameter controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Location</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td>address</td><td>string</td><td>path</td><td>Yes</td><td>The Bitcoin address to query</td></tr><tr><td>page</td><td>integer</td><td>query</td><td>No</td><td>1-based page index for transaction history. Default 1</td></tr><tr><td>pageSize</td><td>integer</td><td>query</td><td>No</td><td>History items per page. Default and maximum is 1000</td></tr><tr><td>from</td><td>integer</td><td>query</td><td>No</td><td>First block height to include when filtering history</td></tr><tr><td>to</td><td>integer</td><td>query</td><td>No</td><td>Last block height to include when filtering history</td></tr><tr><td>details</td><td>string</td><td>query</td><td>No</td><td>Detail level: basic, txids, txslight, or txs. Default txids</td></tr><tr><td>filter</td><td>string</td><td>query</td><td>No</td><td>Filter history by input or output side of the address</td></tr><tr><td>secondary</td><td>string</td><td>query</td><td>No</td><td>Secondary fiat currency code for converted values</td></tr></tbody></table>

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?page=1&pageSize=1000&details=txids'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?page=1&pageSize=1000&details=txids'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?page=1&pageSize=1000&details=txids')

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
    "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
    "balance": "9407625",
    "totalReceived": "45231890",
    "totalSent": "35824265",
    "unconfirmedBalance": "0",
    "unconfirmedTxs": 0,
    "txs": 5,
    "txids": [
        "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "6a3e1f2b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d6e7f8091"
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

| HTTP Status | Message        | Description                                             |
| ----------- | -------------- | ------------------------------------------------------- |
| 400         | Bad request    | The address is malformed or not a valid Bitcoin address |
| 404         | Not found      | No indexed data exists for the requested address        |
| 500         | Internal error | The indexer failed to read address data                 |
