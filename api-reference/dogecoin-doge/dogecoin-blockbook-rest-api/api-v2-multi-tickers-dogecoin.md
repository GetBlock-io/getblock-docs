---
description: >-
  Example code for the api/v2/multi-tickers REST method. Complete guide on how to use the
  api/v2/multi-tickers REST method in the GetBlock Web3 documentation.
---

# api/v2/multi-tickers - Dogecoin

This endpoint returns fiat rate tickers for a comma-separated list of Unix timestamps in a single request.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| timestamp | string | query | Yes | Comma-separated Unix timestamps |
| currency | string | query | No | Fiat currency code |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1789600000,1789700000&currency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1789600000,1789700000&currency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1789600000,1789700000&currency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "ts": 1789603200,
        "rates": {
            "usd": 0.080848895
        }
    },
    {
        "ts": 1789700100,
        "rates": {
            "usd": 0.082758665
        }
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| ts | integer | Timestamp of the rate actually returned for that entry |
| rates | object | Rates keyed by currency code |

Results are returned in the same order as the requested timestamps. Each `ts` is the timestamp of the closest available rate, so it will usually differ from the value requested.

## Use Cases

* **Backfilling**: Fetch rates for many past transactions in one call
* **Reporting**: Build a rate series for a statement period
* **Efficiency**: Replace repeated single-timestamp lookups

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
