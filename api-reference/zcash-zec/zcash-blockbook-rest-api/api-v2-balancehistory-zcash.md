---
description: >-
  Example code for the api/v2/balancehistory REST method. Complete guide on how to use the
  api/v2/balancehistory REST method in the GetBlock Web3 documentation.
---

# api/v2/balancehistory - Zcash

This endpoint returns aggregated balance-change history for a transparent address, extended public key, or descriptor over a time range, optionally with fiat rates for each interval.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | Transparent address, extended public key, or descriptor |
| from | integer | query | No | Unix timestamp lower bound |
| to | integer | query | No | Unix timestamp upper bound |
| fiatcurrency | string | query | No | Fiat currency code to include in rates |
| groupBy | integer | query | No | Aggregation interval in seconds. Default 3600 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?from=1789600000&to=1789700000&fiatcurrency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?from=1789600000&to=1789700000&fiatcurrency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua?from=1789600000&to=1789700000&fiatcurrency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "time": 1789599600,
        "txs": 50,
        "received": "5418386912725",
        "sent": "5522158272750",
        "sentToSelf": "5394301520128",
        "rates": {
            "usd": 1287.9576
        }
    },
    {
        "time": 1789603200,
        "txs": 75,
        "received": "8453765280653",
        "sent": "8490411729794",
        "sentToSelf": "8384777679262",
        "rates": {
            "usd": 1478.55
        }
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| time | integer | Start of the interval as a Unix timestamp |
| txs | integer | Number of transactions in the interval |
| received | string | Total received in the interval, in zatoshis |
| sent | string | Total sent in the interval, in zatoshis |
| sentToSelf | string | Amount returned to the same account, in zatoshis |
| rates | object | Fiat rates for the interval, keyed by currency code |

The array is truncated above; the documented range returns 28 hourly points. Most of what this account appears to receive is its own change returning, as `sentToSelf` shows, so subtract it before treating `received` as incoming value.

## Use Cases

* **Portfolio Charts**: Plot transparent balance movement over time
* **Fiat Valuation**: Value historical movements at the rate of the day
* **Activity Summaries**: Show how active an account was per interval

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
