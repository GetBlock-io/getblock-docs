---
description: >-
  Example code for the bb_getTx JSON-RPC method. Complete guide on how to use
  the bb_getTx JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getTx - Bitcoin

This method returns a normalized transaction by its id, with inputs, outputs, addresses, values, and confirmation data in the indexer's unified schema.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| txid      | string | Yes      | The transaction ID |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b'
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
    "method": "bb_getTx",
    "params": [
        "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
  "version": 2,
  "vin": [
    {
      "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
      "vout": 0,
      "value": "625000000",
      "addresses": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
      ]
    }
  ],
  "vout": [
    {
      "value": "624990000",
      "n": 0,
      "addresses": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
      ],
      "spent": false
    }
  ],
  "blockHash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
  "blockHeight": 830000,
  "confirmations": 152,
  "blockTime": 1706886000,
  "value": "624990000",
  "valueIn": "625000000",
  "fees": "10000"
}
```

## Response Parameters

| Field         | Type    | Description                                      |
| ------------- | ------- | ------------------------------------------------ |
| txid          | string  | Transaction ID                                   |
| vin           | array   | Inputs with resolved addresses and values        |
| vout          | array   | Outputs with addresses, values, and spent status |
| confirmations | integer | Number of confirmations                          |
| fees          | string  | Transaction fee in satoshis                      |

## Use Cases

* **Transaction Views**: Render a transaction with resolved addresses
* **Payment Tracking**: Read values and spent status per output
* **Indexing**: Ingest normalized transactions
* **Accounting**: Read fees and net values

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
