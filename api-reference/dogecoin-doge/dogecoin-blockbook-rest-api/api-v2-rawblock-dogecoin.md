---
description: >-
  Example code for the api/v2/rawblock REST method. Complete guide on how to use the
  api/v2/rawblock REST method in the GetBlock Web3 documentation.
---

# api/v2/rawblock - Dogecoin

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/rawblock/afd10312739304ad5809a454bf723dd2f48fedc08600695fee147a23567be1cf')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "hex": "04016200f6f8f0994db1277087e577a94f91a6ca41df01de87af792bc68316839de8b6..."
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| hex | string | The complete block, serialized and hex-encoded |

{% hint style="warning" %}
The response is a single hex string covering the whole block and is large: the block above serializes to roughly 112,000 hex characters. The value is truncated in this example.

Dogecoin blocks carry an AuxPoW (merge-mining) header after the standard block header, so a parser written for Bitcoin's block format will not read them correctly without handling that extension.
{% endhint %}

## Use Cases

* **Independent Parsing**: Decode a block with your own parser
* **Verification**: Recompute the merkle root or block hash locally
* **Archival**: Store blocks in their canonical serialized form

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
