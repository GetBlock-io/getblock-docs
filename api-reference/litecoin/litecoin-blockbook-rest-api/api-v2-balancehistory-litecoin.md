---
description: >-
  Example code for the api/v2/balancehistory REST method. Complete guide on how to use the
  api/v2/balancehistory REST method in the GetBlock Web3 documentation.
---

# api/v2/balancehistory - Litecoin

This endpoint returns aggregated balance-change history for an address, extended public key, or descriptor over a time range, optionally with fiat rates for each interval.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | Address, extended public key, or descriptor |
| from | integer | query | No | Unix timestamp lower bound |
| to | integer | query | No | Unix timestamp upper bound |
| fiatcurrency | string | query | No | Fiat currency code to include in rates. All available currencies when omitted |
| groupBy | integer | query | No | Aggregation interval in seconds. Default 3600 |
| gap | integer | query | No | Derivation gap limit for xpub inputs |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?from=1789600000&to=1789700000&fiatcurrency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?from=1789600000&to=1789700000&fiatcurrency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/LMRmNEdhi5F43vtW5qPdv1q9s7xsMp4XK5?from=1789600000&to=1789700000&fiatcurrency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
[
    {
        "time": 1789610400,
        "txs": 1,
        "received": "8080253532",
        "sent": "0",
        "sentToSelf": "0",
        "rates": {
            "usd": 51.99346
        }
    },
    {
        "time": 1789696800,
        "txs": 1,
        "received": "8324080358",
        "sent": "0",
        "sentToSelf": "0",
        "rates": {
            "usd": 54.492695
        }
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| time | integer | Start of the interval as a Unix timestamp |
| txs | integer | Number of transactions in the interval |
| received | string | Total received in the interval, in litoshis |
| sent | string | Total sent in the interval, in litoshis |
| sentToSelf | string | Amount sent from the account back to itself, in litoshis |
| rates | object | Fiat rates for the interval, keyed by currency code |

{% hint style="info" %}
Intervals with no activity are omitted rather than returned as zero rows. `sentToSelf` covers change returned to the same address or, for an xpub, movement between addresses of the same wallet; subtract it to avoid counting internal transfers as spending.
{% endhint %}

## Use Cases

* **Portfolio Charts**: Plot balance movement over time
* **Fiat Valuation**: Value historical movements at the rate of the day
* **Tax Reporting**: Summarize inflow and outflow across a period
* **Activity Summaries**: Show how active an account was per interval

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The address or a timestamp parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The indexer failed to build the history |
