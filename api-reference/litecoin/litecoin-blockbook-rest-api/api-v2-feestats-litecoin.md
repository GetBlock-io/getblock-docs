---
description: >-
  Example code for the api/v2/feestats REST method. Complete guide on how to use the
  api/v2/feestats REST method in the GetBlock Web3 documentation.
---

# api/v2/feestats - Litecoin

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3179800'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3179800'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3179800')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txCount": 59,
    "totalFeesSat": "73004",
    "averageFeePerKb": 7175,
    "decilesFeePerKb": [
        0,
        1000,
        1000,
        1062,
        3000,
        3000,
        3000,
        5000,
        7000,
        10014,
        159836
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txCount | integer | Number of transactions counted, excluding the coinbase |
| totalFeesSat | string | Total fees paid in the block, in litoshis |
| averageFeePerKb | number | Average fee rate in litoshis per kilobyte |
| decilesFeePerKb | array | Eleven boundary values describing the fee-rate distribution, from minimum to maximum |

{% hint style="info" %}
`decilesFeePerKb` holds eleven entries, the ten decile boundaries plus the maximum. A leading `0` is normal and reflects zero-fee transactions in the block. The spread is often wide, so the average alone is a poor guide to what a transaction needs to pay.
{% endhint %}

## Use Cases

* **Fee Benchmarking**: Compare a planned fee against what recently confirmed
* **Congestion Analysis**: Track how rates move block to block
* **Rate Cards**: Derive economy and priority tiers from the decile spread

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The block id is not a valid height or hash |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No block matches the requested id |
| 500 | Internal error | The indexer failed to compute fee statistics |
