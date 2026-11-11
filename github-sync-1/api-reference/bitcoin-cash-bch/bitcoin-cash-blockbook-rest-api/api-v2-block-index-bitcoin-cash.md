---
description: >-
  Example code for the api/v2/block-index REST method. Complete guide on how to
  use the api/v2/block-index  REST method in the GetBlock Web3 documentation.
---

# api/v2/block-index - Bitcoin Cash

This endpoint returns the block hash at a given block height. It converts a height into the hash used by hash-based block lookups.

## Parameters

| Parameter   | Type    | Location | Required | Description                    |
| ----------- | ------- | -------- | -------- | ------------------------------ |
| blockHeight | integer | path     | Yes      | Block height on the main chain |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634'
);
console.log(await response.json());
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634')

print(response.json())
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "blockHash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"
}
```

## Response Parameters

| Field     | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| blockHash | string | Hash of the block at the requested height |

## Use Cases

* **Height to Hash**: Resolve a height into a hash for a block lookup
* **Sequential Access**: Walk the chain by height when indexing blocks
* **Checkpoint Checks**: Confirm a height maps to an expected block hash
* **Explorer Links**: Build block detail links from height inputs

## Error Handling

| HTTP Status | Message        | Description                                   |
| ----------- | -------------- | --------------------------------------------- |
| 400         | Bad request    | The block height or hash is malformed         |
| 404         | Not found      | No block matches the requested height or hash |
| 500         | Internal error | The indexer failed to read the block          |
