# bb\_getblock bitcoin

This method returns a block by height or hash, including its metadata and a paged list of the transactions it contains.

## Parameters

| Parameter | Type    | Required | Description                                                |
| --------- | ------- | -------- | ---------------------------------------------------------- |
| blockId   | string  | Yes      | Block height or block hash                                 |
| page      | integer | No       | 1-based page index for the block's transactions. Default 1 |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428?page=1'
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
    "method": "bb_getBlock",
    "params": [
        "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        {
            "page": 1
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
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428?page=1');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block/000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428?page=1')
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
  "hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
  "height": 830000,
  "confirmations": 152,
  "size": 1483920,
  "time": 1706886000,
  "version": 671088644,
  "merkleRoot": "e3a4b5...",
  "nonce": "3722946288",
  "bits": "17034219",
  "difficulty": "75502165623893.83",
  "txCount": 3201,
  "txs": [
    {
      "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
      "value": "625000000"
    }
  ]
}
```

## Response Parameters

| Field         | Type    | Description                             |
| ------------- | ------- | --------------------------------------- |
| hash          | string  | Block hash                              |
| height        | integer | Block height                            |
| confirmations | integer | Number of confirmations                 |
| txCount       | integer | Number of transactions in the block     |
| txs           | array   | Paged list of transactions in the block |

## Use Cases

* **Block Explorers**: Render a block with its indexed transactions
* **Indexing**: Ingest blocks with normalized transaction data
* **Pagination**: Page through a block's transactions
* **Analytics**: Aggregate block-level activity

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
