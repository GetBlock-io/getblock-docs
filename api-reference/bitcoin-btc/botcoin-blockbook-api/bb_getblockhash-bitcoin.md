# bb\_getblockhash bitcoin

This method returns the block hash at a given block height.

## Parameters

| Parameter   | Type    | Required | Description      |
| ----------- | ------- | -------- | ---------------- |
| blockHeight | integer | Yes      | The block height |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/830000'
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
    "method": "bb_getBlockHash",
    "params": [
        830000
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/830000');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/830000')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "blockHash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"
}
```

## Response Parameters

| Field     | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| blockHash | string | The hash of the block at the given height |

## Use Cases

* **Hash Resolution**: Resolve a height to its block hash
* **Pagination**: Walk the chain by height
* **Explorer Links**: Map a height to a linkable hash
* **Snapshotting**: Pin a block hash for consistent reads

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
