---
description: >-
  Example code for the api/v2/block-index REST method. Complete guide on how to use the
  api/v2/block-index REST method in the GetBlock Web3 documentation.
---

# api/v2/block-index - Zcash

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3487790'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3487790'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/3487790')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "blockHash": "00000000000f3c0bdf81aba0bf081c5c4882975cbc0a8ce35e70ddb5f265f1b1"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| blockHash | string | Hash of the block at the requested height |

## Use Cases

* **Height to Hash**: Resolve a height before fetching a block by hash
* **Chain Checks**: Confirm which block occupies a height after a reorganization

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
