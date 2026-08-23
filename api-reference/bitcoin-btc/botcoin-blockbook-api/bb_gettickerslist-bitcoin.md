# bb\_gettickerslist bitcoin

This method returns the list of fiat currencies for which rates are available at a given timestamp.

## Parameters

| Parameter | Type    | Required | Description                         |
| --------- | ------- | -------- | ----------------------------------- |
| timestamp | integer | No       | Unix timestamp; latest when omitted |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1706886000'
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
            "timestamp": 1706886000
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
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1706886000');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1706886000')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
  "ts": 1706886000,
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

| Field                 | Type    | Description                     |
| --------------------- | ------- | ------------------------------- |
| ts                    | integer | Timestamp of the list           |
| available\_currencies | array   | Currencies with available rates |

## Use Cases

* **Currency Menus**: Populate a fiat currency selector
* **Availability Checks**: Confirm a currency has rates
* **Localization**: Offer supported local currencies
* **Tooling**: Discover available rate currencies

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
