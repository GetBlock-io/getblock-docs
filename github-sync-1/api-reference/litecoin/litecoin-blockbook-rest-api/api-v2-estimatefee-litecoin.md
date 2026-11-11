---
description: >-
  Example code for the api/v2/estimatefee REST method. Complete guide on how to use the
  api/v2/estimatefee REST method in the GetBlock Web3 documentation.
---

# api/v2/estimatefee - Litecoin

This endpoint returns the backend fee estimate for a target number of blocks to confirmation. The result is a fee rate in LTC per kilobyte.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blocks | integer | path | Yes | Confirmation target in blocks |
| conservative | boolean | query | No | Use conservative smart fee estimation. Default true |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "0.00000994"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| result | string | Estimated fee rate as a decimal amount in LTC per kilobyte |

{% hint style="info" %}
The rate is quoted per kilobyte in whole LTC. Multiply by 100,000,000 and divide by 1000 to get litoshis per byte: the `0.00000994` above is roughly 0.99 litoshis/byte. The WebSocket [estimateFee](../litecoin-blockbook-websocket-api/estimatefee-litecoin.md) method returns the same estimate as an integer count of litoshis per kilobyte instead.
{% endhint %}

## Use Cases

* **Fee Selection**: Choose a rate matching the confirmation speed a payment needs
* **Cost Preview**: Show the user the fee before signing
* **Batch Planning**: Size a consolidation against the current rate

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The confirmation target is not a valid integer |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The node failed to return an estimate |
