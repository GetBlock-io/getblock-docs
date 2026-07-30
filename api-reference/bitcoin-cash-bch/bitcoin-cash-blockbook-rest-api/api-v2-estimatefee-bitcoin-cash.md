---
description: >-
  Example code for the api/v2/estimatefee REST method. Complete guide on how to
  use the api/v2/estimatefee REST method in the GetBlock Web3 documentation.
---

# api/v2/estimatefee - Bitcoin Cash

This endpoint returns the backend fee estimate for a target number of blocks to confirmation. The result is a fee rate in BCH per kilobyte.

## Parameters

| Parameter    | Type    | Location | Required | Description                                     |
| ------------ | ------- | -------- | -------- | ----------------------------------------------- |
| blocks       | integer | path     | Yes      | Confirmation target in blocks                   |
| conservative | boolean | query    | No       | Use conservative fee estimation where supported |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/estimatefee/6?conservative=true')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "result": "0.00001000"
}
```

## Response Parameters

| Field  | Type   | Description                                                |
| ------ | ------ | ---------------------------------------------------------- |
| result | string | Estimated fee rate as a decimal amount in BCH per kilobyte |

## Use Cases

* **Fee Selection**: Choose a fee rate for a target confirmation time
* **Wallet UX**: Offer fast, normal, and economy tiers from different targets
* **Cost Estimation**: Estimate the cost of a transaction before building it
* **Conservative Mode**: Request a higher-confidence estimate for urgent sends

## Error Handling

| HTTP Status | Message        | Description                                       |
| ----------- | -------------- | ------------------------------------------------- |
| 400         | Bad request    | The confirmation target is not a positive integer |
| 404         | Not found      | The backend returned no estimate for the target   |
| 500         | Internal error | The backend fee estimator failed                  |
