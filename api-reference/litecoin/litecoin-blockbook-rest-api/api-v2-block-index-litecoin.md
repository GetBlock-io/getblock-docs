---
description: >-
  Example code for the api/v2/block-index REST method. Complete guide on how to use the
  api/v2/block-index REST method in the GetBlock Web3 documentation.
---

# api/v2/block-index - Litecoin

This endpoint returns the block hash at a given block height on the main chain.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockHeight | integer | path | Yes | Block height on the main chain |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3179800'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3179800'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3179800')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "blockHash": "0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| blockHash | string | Hash of the block at the requested height |

## Use Cases

* **Height to Hash**: Resolve a height before fetching a block by hash
* **Chain Checks**: Confirm which block occupies a height after a reorganization
* **Indexing**: Walk heights sequentially while building a local index

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The height is not a valid integer |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | The height is beyond the current chain tip |
| 500 | Internal error | The indexer failed to resolve the height |
