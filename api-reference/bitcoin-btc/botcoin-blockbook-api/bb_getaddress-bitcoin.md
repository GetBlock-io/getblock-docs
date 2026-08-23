---
description: >-
  Example code for the bb_getAddress JSON-RPC method. Complete guide on how to
  use the bb_getAddress JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getAddress - Bitcoin

This method returns balance and transaction data for a single address. The details option controls how much data is returned, from a balance-only summary to full transaction objects.

## Parameters

| Parameter | Type    | Required | Description                                       |
| --------- | ------- | -------- | ------------------------------------------------- |
| address   | string  | Yes      | The address to query                              |
| details   | string  | No       | Level of detail: basic, txids, txs. Default txids |
| page      | integer | No       | 1-based page index. Default 1                     |
| pageSize  | integer | No       | Transactions per page. Default 1000               |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?details=txids&page=1&pageSize=1000'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getAddress",
    "params": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        {
            "details": "txids",
            "page": 1,
            "pageSize": 1000
        }
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?details=txids&page=1&pageSize=1000');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/address/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?details=txids&page=1&pageSize=1000')
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
  "balance": "150000000",
  "totalReceived": "500000000",
  "totalSent": "350000000",
  "unconfirmedBalance": "0",
  "unconfirmedTxs": 0,
  "txs": 42,
  "txids": [
    "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
  ]
}
```

## Response Parameters

| Field         | Type    | Description                            |
| ------------- | ------- | -------------------------------------- |
| address       | string  | The queried address                    |
| balance       | string  | Confirmed balance in satoshis          |
| totalReceived | string  | Total received in satoshis             |
| totalSent     | string  | Total sent in satoshis                 |
| txs           | integer | Number of transactions                 |
| txids         | array   | Transaction IDs, when details is txids |

## Use Cases

* **Balance Display**: Show an address balance and totals
* **Transaction History**: List an address's transactions
* **Wallets**: Read address state without a full node
* **Monitoring**: Watch an address for activity

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
