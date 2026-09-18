---
description: >-
  Example code for the api/v2/tickers-list REST method. Complete guide on how to use the
  api/v2/tickers-list REST method in the GetBlock Web3 documentation.
---

# api/v2/tickers-list - Litecoin

This endpoint returns the currencies for which the indexer has rate data available at a given timestamp.

## Parameters

| Parameter | Type | Location | Required | Description |
| --- | --- | --- | --- | --- |
| timestamp | integer | query | Yes | Unix timestamp for the requested currency list |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789699786'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789699786'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789699786')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1789699800,
    "available_currencies": [
        "aed",
        "ars",
        "aud",
        "bch",
        "bdt",
        "bhd",
        "bits",
        "bmd",
        "bnb",
        "brl",
        "btc",
        "cad"
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| ts | integer | Timestamp of the rate set actually returned |
| available_currencies | array | Currency codes with rate data at that timestamp |

The list is alphabetical and truncated above. At the time of writing the endpoint returns 62 currencies for Litecoin, mixing fiat codes with crypto denominations such as `btc`, `bch`, and `bnb`.

## Use Cases

* **Currency Pickers**: Populate a selector with currencies that actually resolve
* **Validation**: Check a currency is supported before requesting a rate
* **Coverage Checks**: Confirm rate availability for a historical date

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | The timestamp is missing or malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 500 | Internal error | The indexer failed to read rate data |
