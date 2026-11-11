---
description: >-
  Example code for the api/v2/feestats REST method. Complete guide on how to use
  the api/v2/feestats REST method in the GetBlock Web3 documentation.
---

# api/v2/feestats - Dash

This endpoint returns fee statistics for the transactions in a single block, selected by height or hash. The load grows with the number of transactions in the block.

## Parameters

| Parameter | Type   | Location | Required | Description                |
| --------- | ------ | -------- | -------- | -------------------------- |
| blockId   | string | path     | Yes      | Block height or block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/2540670'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/2540670'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/feestats/2540670')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "txCount": 30,
    "totalFeesSat": "226187",
    "averageFeePerKb": 6441,
    "decilesFeePerKb": [
        0,
        0,
        1000,
        1002,
        1003,
        1004,
        1008,
        2008,
        6735,
        32204,
        64233
    ]
}
```

## Response Parameters

| Field           | Type    | Description                                      |
| --------------- | ------- | ------------------------------------------------ |
| txCount         | integer | Number of transactions in the block              |
| totalFeesSat    | string  | Total fees in the block, in duffs             |
| averageFeePerKb | number  | Average fee rate in duffs per kilobyte        |
| decilesFeePerKb | array   | Fee-rate deciles across the block's transactions |

## Use Cases

* **Fee Market Analysis**: Study the fee distribution within recent blocks
* **Fee Modeling**: Use deciles to model competitive fee rates
* **Dashboards**: Chart average and total fees per block
* **Historical Study**: Compare fee statistics across a range of blocks

## Error Handling

| HTTP Status | Message        | Description                                   |
| ----------- | -------------- | --------------------------------------------- |
| 400         | Bad request    | The block height or hash is malformed         |
| 404         | Not found      | No block matches the requested height or hash |
| 500         | Internal error | The indexer failed to read the block          |
