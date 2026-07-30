---
description: >-
  Example code for the api/v2/tickers REST method. Complete guide on how to use
  the api/v2/tickers REST method in the GetBlock Web3 documentation.
---

# api/v2/tickers/- Bitcoin Cash

This endpoint returns current or historical fiat exchange rates for Bitcoin Cash. A timestamp or block selects historical rates.

## Parameters

| Parameter | Type    | Location | Required | Description                                                        |
| --------- | ------- | -------- | -------- | ------------------------------------------------------------------ |
| currency  | string  | query    | No       | Fiat currency code. When omitted, all available rates are returned |
| timestamp | integer | query    | No       | Unix timestamp for historical rates                                |
| block     | string  | query    | No       | Block height or hash whose time is used for historical rates       |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1785418362,
    "rates": {
        "usd": 211.79
    }
}
```

## Response Parameters

| Field | Type    | Description                           |
| ----- | ------- | ------------------------------------- |
| ts    | integer | Timestamp of the returned rates       |
| rates | object  | Map of fiat currency codes to rates   |
| error | string  | Error text when a rate is unavailable |

## Use Cases

* **Fiat Display**: Show a BCH amount in a user's local currency
* **Historical Valuation**: Read the rate at a past timestamp or block
* **Invoicing**: Convert a fiat-priced invoice into BCH at the current rate
* **Reporting**: Attach fiat values to transaction histories

## Error Handling

| HTTP Status | Message        | Description                                 |
| ----------- | -------------- | ------------------------------------------- |
| 400         | Bad request    | A query parameter has an invalid value      |
| 404         | Not found      | No rate data exists for the requested query |
| 500         | Internal error | The indexer failed to read rate data        |
