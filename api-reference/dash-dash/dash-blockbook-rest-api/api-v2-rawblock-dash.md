---
description: >-
  Example code for the api/v2/rawblockREST method. Complete guide on how to use
  the api/v2/rawblock REST method in the GetBlock Web3 documentation.
---

# api/v2/rawblock - Dash

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/00000000000000007409ab5be18f66b1f68795ff81d3b3b1d592574fae07c372')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hex": "0000002024c93230b6263646258d191468812edcde952f939717de4d0e00000000000000baa7eedf..."
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
