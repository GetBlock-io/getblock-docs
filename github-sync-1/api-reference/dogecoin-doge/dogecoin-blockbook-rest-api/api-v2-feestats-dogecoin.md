---
description: >-
  Example code for the api/v2/feestats REST method. Complete guide on how to use the
  api/v2/feestats REST method in the GetBlock Web3 documentation.
---

# api/v2/feestats - Dogecoin

This endpoint returns fee statistics for the transactions contained in one block, including the average rate and the decile distribution.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockId | string | path | Yes | Block height or block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/6378930'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/6378930'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/6378930')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txCount": 69,
    "totalFeesSat": "2346629541",
    "averageFeePerKb": 65422296,
    "decilesFeePerKb": [
        1000000,
        1513333,
        2000000,
        2010471,
        10044444,
        14285714,
        32000000,
        36321989,
        60266666,
        202666666,
        552444444
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txCount | integer | Number of transactions counted, excluding the coinbase |
| totalFeesSat | string | Total fees paid in the block, in koinu |
| averageFeePerKb | number | Average fee rate in koinu per kilobyte |
| decilesFeePerKb | array | Eleven boundary values describing the fee-rate distribution, from minimum to maximum |

{% hint style="info" %}
Rates here are in koinu per kilobyte, so the figures are large: `65422296` is roughly 0.654 DOGE per kilobyte. The spread across deciles is wide — from 0.01 to 5.5 DOGE per kilobyte in the block above — so the average alone is a poor guide to what a transaction needs to pay.
{% endhint %}

## Use Cases

* **Fee Benchmarking**: Compare a planned fee against what recently confirmed
* **Congestion Analysis**: Track how rates move block to block
* **Rate Cards**: Derive economy and priority tiers from the decile spread

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
