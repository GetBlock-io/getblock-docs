---
description: >-
  Example code for the bb_getBlockHash JSON-RPC method. Complete guide on how to
  use the bb_getBlockHash JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getBlockHash - Bitcoin Cash

This method returns the block hash at a given block height. It converts a height into the hash used by hash-based block lookups.

## Parameters

| Parameter | Type    | Required | Description                    |
| --------- | ------- | -------- | ------------------------------ |
| height    | integer | Yes      | Block height on the main chain |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getBlockHash",
    "params": [
        684634
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/block-index/684634')

print(response.json())
```
{% endcode %}
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

| Error Code   | Message        | Description                                   |
| ------------ | -------------- | --------------------------------------------- |
| 400 / -32602 | Bad request    | The block height or hash is malformed         |
| 404 / -32603 | Not found      | No block matches the requested height or hash |
| 500 / -32603 | Internal error | The indexer failed to read the block          |
