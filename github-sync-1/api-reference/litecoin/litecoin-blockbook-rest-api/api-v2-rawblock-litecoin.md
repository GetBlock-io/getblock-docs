---
description: >-
  Example code for the api/v2/rawblock REST method. Complete guide on how to use the
  api/v2/rawblock REST method in the GetBlock Web3 documentation.
---

# api/v2/rawblock - Litecoin

This endpoint returns the raw serialized hex of a block, selected by height or hash.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockId | string | path | Yes | Block height or block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/0fc24234c2379a5262023a4b4a653ff3a11df6bb1c12851aff820d27ea02b0b6')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hex": "1400002041399939802e19d56942bdb74571b1af8fcf0ae3f8be1eb20bbfa19974b9a9e82bd23b23..."
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hex | string | The complete block, serialized and hex-encoded |

{% hint style="warning" %}
The response is a single hex string covering the whole block and is large: the block above serializes to roughly 181,000 hex characters. The value is truncated in this example.
{% endhint %}

## Use Cases

* **Independent Parsing**: Decode a block with your own parser
* **Verification**: Recompute the merkle root or block hash locally
* **Archival**: Store blocks in their canonical serialized form

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The block id is not a valid height or hash |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No block matches the requested id |
| 500 | Internal error | The indexer failed to read the block |
