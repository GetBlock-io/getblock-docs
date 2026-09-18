---
description: >-
  Example code for the api/v2/tickers REST method. Complete guide on how to use the
  api/v2/tickers REST method in the GetBlock Web3 documentation.
---

# api/v2/tickers - Dogecoin

This endpoint returns current or historical fiat exchange rates for Dogecoin. A timestamp or block selects historical rates; without either, the latest available rates are returned.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| currency | string | query | No | Fiat currency code. All available currencies when omitted |
| timestamp | integer | query | No | Unix timestamp for historical rates |
| block | string | query | No | Block height or hash whose timestamp selects the rate |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1789706100,
    "rates": {
        "usd": 0.083795
    }
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| ts | integer | Timestamp of the rate actually returned |
| rates | object | Rates keyed by currency code |

{% hint style="info" %}
`ts` is the timestamp of the rate actually returned, which can differ from the one requested when no exact match exists. A rate of `-1` marks a currency that is unavailable or invalid for that timestamp.
{% endhint %}

## Use Cases

* **Fiat Display**: Show a DOGE amount in a user's local currency
* **Historical Valuation**: Value a past transaction at the rate of its block
* **Invoicing**: Convert a fiat-priced invoice into DOGE at the current rate

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
