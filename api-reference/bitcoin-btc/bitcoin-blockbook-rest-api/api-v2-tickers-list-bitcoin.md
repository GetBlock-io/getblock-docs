# api v2 tickers list bitcoin

This endpoint returns the fiat currencies for which the indexer has rate data available at a given timestamp.

## Parameters

| Parameter | Type    | Location | Required | Description                                          |
| --------- | ------- | -------- | -------- | ---------------------------------------------------- |
| timestamp | integer | query    | Yes      | Unix timestamp for the requested currency list       |
| token     | string  | query    | No       | Token symbol or contract key for token-specific data |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1617180599'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1617180599'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1617180599')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1617180599,
    "available_currencies": [
        "usd",
        "eur",
        "gbp",
        "jpy",
        "btc"
    ]
}
```

## Response Parameters

| Field                 | Type    | Description                            |
| --------------------- | ------- | -------------------------------------- |
| ts                    | integer | Timestamp the currency list applies to |
| available\_currencies | array   | Currency codes with rate data          |
| error                 | string  | Error text when no list is available   |

## Use Cases

* **Currency Pickers**: Populate a currency selector with supported codes
* **Rate Availability**: Check which currencies have data before requesting rates
* **Localization**: Offer only currencies the indexer can price
* **Validation**: Confirm a requested currency is supported at a timestamp

## Error Handling

| HTTP Status | Message        | Description                                 |
| ----------- | -------------- | ------------------------------------------- |
| 400         | Bad request    | A query parameter has an invalid value      |
| 404         | Not found      | No rate data exists for the requested query |
| 500         | Internal error | The indexer failed to read rate data        |
