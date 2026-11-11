---
description: >-
  Example code for the api/v2/rawblockREST method. Complete guide on how to use
  the api/v2/rawblock REST method in the GetBlock Web3 documentation.
---

# api/v2/rawblock - Bitcoin Cash

This endpoint returns the raw serialized hex of a block, selected by height or hash. The payload grows with block size.

## Parameters

| Parameter | Type   | Location | Required | Description                |
| --------- | ------ | -------- | -------- | -------------------------- |
| blockId   | string | path     | Yes      | Block height or block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hex": "00000020dd93f73898f4cde11a47d7a06244c5eed1f52d3718a3d3010000000000000000..."
}
```

## Response Parameters

| Field | Type   | Description                          |
| ----- | ------ | ------------------------------------ |
| hex   | string | Raw serialized block as a hex string |

## Use Cases

* **Full Block Parsing**: Retrieve raw block bytes for local decoding
* **Archival**: Store raw blocks for an independent index
* **Verification**: Recompute the block hash from its serialized form
* **Cross-Tool Import**: Feed raw hex into libraries that parse block data

## Error Handling

| HTTP Status | Message        | Description                                   |
| ----------- | -------------- | --------------------------------------------- |
| 400         | Bad request    | The block height or hash is malformed         |
| 404         | Not found      | No block matches the requested height or hash |
| 500         | Internal error | The indexer failed to read the block          |
