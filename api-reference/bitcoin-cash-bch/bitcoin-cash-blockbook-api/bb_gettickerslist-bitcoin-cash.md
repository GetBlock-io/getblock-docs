---
description: >-
  Example code for the bb_getTickersList JSON-RPC method. Complete guide on how
  to use the bb_getTickersList JSON-RPC method in the GetBlock Web3
  documentation.
---

# bb\_getTickersList - Bitcoin Cash

This method returns the fiat currencies for which the indexer has rate data available at a given timestamp.

## Parameters

| Parameter | Type    | Required | Description                                    |
| --------- | ------- | -------- | ---------------------------------------------- |
| timestamp | integer | Yes      | Unix timestamp for the requested currency list |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1617180599'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "bb_getTickersList",
    "params": [
        {
            "timestamp": 1617180599
        }
    ],
    "id": "getblock.io"
}'
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

| Field                 | Type    | Description                                   |
| --------------------- | ------- | --------------------------------------------- |
| ts                    | integer | Timestamp the currency list applies to        |
| available\_currencies | array   | Fiat and crypto currency codes with rate data |
| error                 | string  | Error text when no list is available          |

## Use Cases

* **Currency Pickers**: Populate a currency selector with supported codes
* **Rate Availability**: Check which currencies have data before requesting rates
* **Localization**: Offer only currencies the indexer can price
* **Validation**: Confirm a requested currency is supported at a timestamp

## Error Handling

| Error Code   | Message        | Description                                                 |
| ------------ | -------------- | ----------------------------------------------------------- |
| 400 / -32602 | Bad request    | A query parameter has an invalid value                      |
| 404 / -32603 | Not found      | No rate data exists for the requested currency or timestamp |
| 500 / -32603 | Internal error | The indexer failed to read rate data                        |
