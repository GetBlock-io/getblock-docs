---
description: >-
  Example code for the rest/blockhashbyheight REST endpoint. Complete guide on how to use the
  rest/blockhashbyheight REST endpoint in the GetBlock Web3 documentation.
---

# rest/blockhashbyheight - Bitcoin

This endpoint returns the hash of the block at a given height on the main chain.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| height | integer | path | Yes | Block height on the main chain |
| format | string | path | Yes | Response format appended to the height: `json`, `hex`, or `bin` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/blockhashbyheight/967816.json'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/blockhashbyheight/967816.json'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/rest/blockhashbyheight/967816.json')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "blockhash": "000000000000000000019c481c042c0625abe1dc04fe26795e2e24b152791e5f"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| blockhash | string | Hash of the block at the requested height |

{% hint style="info" %}
Every other REST path takes a block **hash**, so this is usually the first call when you only know a height.
{% endhint %}

## Use Cases

* **Height to Hash**: Resolve a height before calling an endpoint that takes a hash
* **Chain Checks**: Confirm which block occupies a height after a reorganization
* **Indexing**: Walk heights sequentially while building a local index

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The path is malformed, or a hash or height is invalid |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No object matches the requested hash, height, or outpoint |
| 500 | Internal error | The node failed to process the request |
