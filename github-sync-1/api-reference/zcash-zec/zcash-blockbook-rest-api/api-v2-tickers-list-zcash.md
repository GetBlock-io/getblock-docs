---
description: >-
  Example code for the api/v2/tickers-list REST method. Complete guide on how to use the
  api/v2/tickers-list REST method in the GetBlock Web3 documentation.
---

# api/v2/tickers-list - Zcash

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
curl --location --request GET 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789700000'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" overflow="wrap" %}
```javascript
const response = await fetch(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789700000'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" overflow="wrap" %}
```python
import requests

response = requests.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/api/v2/tickers-list/?timestamp=1789700000')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1789700100,
    "available_currencies": [
        "aed",
        "ars",
        "aud",
        "bdt",
        "bhd",
        "bmd",
        "brl",
        "btc",
        "cad",
        "chf"
    ]
}
```

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| ts | integer | Timestamp of the rate set actually returned |
| available_currencies | array | Currency codes with rate data at that timestamp |

The list is alphabetical and truncated above. At the time of writing the endpoint returns 46 currencies for Zcash, fewer than the 62 returned for Bitcoin, Litecoin, and Dogecoin, so check availability rather than assuming parity with another chain.

## Use Cases

* **Currency Pickers**: Populate a selector with currencies that actually resolve
* **Validation**: Check a currency is supported before requesting a rate

## Error Handling

| HTTP Status | Message | Description |
| --- | --- | --- |
| 400 | Bad request | A path or query parameter is malformed |
| 403 | Forbidden | Missing or invalid ACCESS-TOKEN |
| 404 | Not found | No indexed data exists for the request |
| 500 | Internal error | The indexer failed to process the request |
