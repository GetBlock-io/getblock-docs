# bb\_gettickers bitcoin

This method returns fiat exchange rates for the coin at a given timestamp, or the latest rates when no timestamp is supplied.

## Parameters

| Parameter | Type    | Required | Description                                              |
| --------- | ------- | -------- | -------------------------------------------------------- |
| currency  | string  | No       | Fiat currency to return; returns all when omitted        |
| timestamp | integer | No       | Unix timestamp for historical rates; latest when omitted |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
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
    "method": "bb_getTickers",
    "params": [
        {
            "currency": "usd"
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
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
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
  "ts": 1706886000,
  "rates": {
    "usd": 68000.12
  }
}
```

## Response Parameters

| Field | Type    | Description                  |
| ----- | ------- | ---------------------------- |
| ts    | integer | Timestamp of the rates       |
| rates | object  | Fiat rates keyed by currency |

## Use Cases

* **Fiat Conversion**: Convert balances to fiat
* **Price Display**: Show the current coin price
* **Historical Rates**: Read rates at a past timestamp
* **Reporting**: Value holdings for reports

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
