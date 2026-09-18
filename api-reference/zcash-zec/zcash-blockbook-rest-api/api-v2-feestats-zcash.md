---
description: >-
  Example code for the api/v2/feestats REST method. Complete guide on how to use the
  api/v2/feestats REST method in the GetBlock Web3 documentation.
---

# api/v2/feestats - Zcash

This endpoint returns fee statistics for the transactions in one block, including the average rate and the decile distribution.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| blockId | string | path | Yes | Block height or block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3487790'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3487790'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/3487790')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txCount": 19,
    "totalFeesSat": "1240422735",
    "averageFeePerKb": 10205992,
    "decilesFeePerKb": [
        13681,
        23344,
        41493,
        42130,
        47393,
        48309,
        106288,
        156774,
        403863,
        13058268,
        172148127
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| txCount | integer | Number of transactions counted |
| totalFeesSat | string | Total fees across the counted transactions, in zatoshis |
| averageFeePerKb | number | Average fee rate in zatoshis per kilobyte |
| decilesFeePerKb | array | Eleven boundary values describing the fee-rate distribution |

{% hint style="warning" %}
Treat these figures with caution on Zcash. They are computed from the normalized schema, which cannot see shielded value, so shielded transactions contribute a fee of zero. The block above holds 32 transactions but only 19 are counted.

Zcash does not price transactions by a fee market: ZIP-317 sets a conventional fee of 5,000 zatoshis per logical action, with a minimum of two actions. Size fees from that rule rather than from these statistics.
{% endhint %}

## Use Cases

* **Fee Benchmarking**: Compare a planned fee against recently confirmed transactions
* **Congestion Analysis**: Track how fees move block to block

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
