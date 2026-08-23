# bb\_getbalancehistory bitcoin

This method returns aggregated balance-change history for an address, extended public key, or descriptor over a time range. Results are grouped into intervals and can include fiat rates.

## Parameters

| Parameter    | Type    | Required | Description                                    |
| ------------ | ------- | -------- | ---------------------------------------------- |
| address      | string  | Yes      | Address, extended public key, or descriptor    |
| from         | integer | No       | Start Unix timestamp                           |
| to           | integer | No       | End Unix timestamp                             |
| fiatcurrency | string  | No       | Fiat currency for rate conversion, such as usd |
| groupBy      | integer | No       | Interval in seconds to group by. Default 3600  |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?from=1706800000&to=1706900000&fiatcurrency=usd'
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
    "method": "bb_getBalanceHistory",
    "params": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        {
            "from": 1706800000,
            "to": 1706900000,
            "fiatcurrency": "usd"
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
const response = await fetch('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?from=1706800000&to=1706900000&fiatcurrency=usd');
const data = await response.json();
console.log(data);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq?from=1706800000&to=1706900000&fiatcurrency=usd')
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
  {
    "time": 1706886000,
    "txs": 2,
    "received": "100000000",
    "sent": "0",
    "sentToSelf": "0",
    "rates": {
      "usd": 68000.12
    }
  }
]
```

## Response Parameters

| Field    | Type    | Description                                  |
| -------- | ------- | -------------------------------------------- |
| time     | integer | Interval start Unix timestamp                |
| txs      | integer | Number of transactions in the interval       |
| received | string  | Amount received in the interval, in satoshis |
| sent     | string  | Amount sent in the interval, in satoshis     |
| rates    | object  | Fiat rates at the interval, when requested   |

## Use Cases

* **Portfolio Charts**: Chart balance changes over time
* **Tax Reporting**: Aggregate activity with fiat rates
* **Accounting**: Summarize inflows and outflows
* **Analytics**: Study address activity patterns

## Error Handling

| HTTP Status | Message      | Description                               |
| ----------- | ------------ | ----------------------------------------- |
| 400         | Bad request  | A path or query parameter is malformed    |
| 403         | Forbidden    | Missing or invalid ACCESS-TOKEN           |
| 404         | Not found    | The requested resource does not exist     |
| 500         | Server error | The indexer failed to process the request |
