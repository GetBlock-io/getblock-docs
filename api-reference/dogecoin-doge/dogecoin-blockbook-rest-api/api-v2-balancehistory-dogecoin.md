---
description: >-
  Example code for the api/v2/balancehistory REST method. Complete guide on how to use the
  api/v2/balancehistory REST method in the GetBlock Web3 documentation.
---

# api/v2/balancehistory - Dogecoin

This endpoint returns aggregated balance-change history for an address, extended public key, or descriptor over a time range, optionally with fiat rates for each interval.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| address | string | path | Yes | Address, extended public key, or descriptor |
| from | integer | query | No | Unix timestamp lower bound |
| to | integer | query | No | Unix timestamp upper bound |
| fiatcurrency | string | query | No | Fiat currency code to include in rates |
| groupBy | integer | query | No | Aggregation interval in seconds. Default 3600 |
| gap | integer | query | No | Derivation gap limit for xpub inputs |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?from=1789600000&to=1789710000&fiatcurrency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?from=1789600000&to=1789710000&fiatcurrency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/balancehistory/DRv9o4XUK1DhNiuKPadPwkQNkPgtcniBFF?from=1789600000&to=1789710000&fiatcurrency=usd')

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
        "txs": 18,
        "received": "3550876310328800",
        "sent": "3569084648638027",
        "sentToSelf": "3550791208486688",
        "rates": {
            "usd": 0.079924904
        }
    },
    {
        "time": 1789603200,
        "txs": 15,
        "received": "4089740795865158",
        "sent": "4107600440042183",
        "sentToSelf": "4089289168264050",
        "rates": {
            "usd": 0.083795
        }
    }
]
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| time | integer | Start of the interval as a Unix timestamp |
| txs | integer | Number of transactions in the interval |
| received | string | Total received in the interval, in koinu |
| sent | string | Total sent in the interval, in koinu |
| sentToSelf | string | Amount sent from the account back to itself, in koinu |
| rates | object | Fiat rates for the interval, keyed by currency code |

The array is truncated above. With `groupBy` left at its 3600-second default, the documented range returns 30 hourly points.

{% hint style="info" %}
Note how close `sentToSelf` is to `received` on this account: most of what it appears to receive is its own change returning. Subtract `sentToSelf` before treating `received` as incoming value, or an exchange hot wallet will look many times busier than it is.
{% endhint %}

## Use Cases

* **Portfolio Charts**: Plot balance movement over time
* **Fiat Valuation**: Value historical movements at the rate of the day
* **Tax Reporting**: Summarize inflow and outflow across a period
* **Activity Summaries**: Show how active an account was per interval

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
