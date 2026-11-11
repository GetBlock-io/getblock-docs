---
description: >-
  Example code for the api/v2/estimatefee REST method. Complete guide on how to use the
  api/v2/estimatefee REST method in the GetBlock Web3 documentation.
---

# api/v2/estimatefee - Dogecoin

This endpoint returns the backend fee estimate for a target number of blocks to confirmation. The result is a fee rate in DOGE per kilobyte.

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
    "result": "0.0100255"
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| result | string | Estimated fee rate as a decimal amount in DOGE per kilobyte |

{% hint style="warning" %}
**A one-block target has no estimate on Dogecoin and returns `-1`.** The node's `estimatesmartfee` reports `-1` when it lacks the data to estimate a target, and Blockbook passes that through unchanged:

| Target      | Result       |
| ----------- | ------------ |
| `blocks=1`  | `-1`         |
| `blocks=2`  | `0.50318115` |
| `blocks=6`  | `0.01002472` |
| `blocks=12` | `0.01002472` |

Always check that the estimate is positive before using it. A client that multiplies `-1` by a transaction size produces a negative fee and builds an invalid transaction. The WebSocket [estimateFee](../dogecoin-blockbook-websocket-api/estimatefee-dogecoin.md) method surfaces the same condition as `feePerUnit: "-100000000"`, the `-1` scaled to koinu.

Note also how steep the curve is: a two-block target costs roughly fifty times a six-block target. Do not treat a fast target as a small premium on Dogecoin.
{% endhint %}

Dogecoin's fees are large in absolute terms compared with other UTXO chains: `0.0100255` DOGE per kilobyte is roughly 1,002,550 koinu per kilobyte. This is normal and reflects the chain's low unit value, not a misconfigured node.

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
