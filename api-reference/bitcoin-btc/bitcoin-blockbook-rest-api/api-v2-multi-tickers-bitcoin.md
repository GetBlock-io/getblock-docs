# api v2 multi tickers bitcoin

This endpoint returns fiat rate tickers for a comma-separated list of Unix timestamps. Work and payload grow with the number of timestamps requested.

## Parameters

| Parameter | Type   | Location | Required | Description                                           |
| --------- | ------ | -------- | -------- | ----------------------------------------------------- |
| timestamp | string | query    | Yes      | Comma-separated Unix timestamps                       |
| currency  | string | query    | No       | Fiat currency code to return                          |
| token     | string | query    | No       | Token symbol or contract key for token-specific rates |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1617180599,1617184199&currency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1617180599,1617184199&currency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/multi-tickers/?timestamp=1617180599,1617184199&currency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "ts": 1617180599,
        "rates": {
            "usd": 512.34
        }
    },
    {
        "ts": 1617184199,
        "rates": {
            "usd": 514.1
        }
    }
]
```

## Response Parameters

| Field | Type    | Description                                      |
| ----- | ------- | ------------------------------------------------ |
| ts    | integer | Timestamp of the returned rate entry             |
| rates | object  | Map of currency codes to rates for the timestamp |

## Use Cases

* **Batch Valuation**: Fetch rates for many points in one request
* **History Backfill**: Attach fiat values across a transaction history at once
* **Charting**: Build a fiat price series from multiple timestamps
* **Reporting**: Value a period of activity with fewer requests

## Error Handling

| HTTP Status | Message        | Description                                 |
| ----------- | -------------- | ------------------------------------------- |
| 400         | Bad request    | A query parameter has an invalid value      |
| 404         | Not found      | No rate data exists for the requested query |
| 500         | Internal error | The indexer failed to read rate data        |
