---
description: >-
  Example code for the bb_getTickers JSON-RPC method. Complete guide on how to
  use the bb_getTickers JSON-RPC method in the GetBlock Web3 documentation.
---

# bb\_getTickers - Bitcoin Cash

This method returns current or historical fiat exchange rates for Bitcoin Cash as tracked by the indexer. A timestamp or block selects historical rates.

## Parameters

| Parameter | Type    | Required | Description                                                        |
| --------- | ------- | -------- | ------------------------------------------------------------------ |
| currency  | string  | No       | Fiat currency code. When omitted, all available rates are returned |
| timestamp | integer | No       | Unix timestamp for historical rates                                |
| block     | string  | No       | Block height or hash whose time is used for historical rates       |

## Request

{% tabs %}
{% tab title="cURL (REST)" %}
{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
```
{% endcode %}
{% endtab %}

{% tab title="cURL (JSON-RPC)" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd'
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.get('https://go.getblock.io/<ACCESS-TOKEN>/api/v2/tickers/?currency=usd')

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "ts": 1617180599,
    "rates": {
        "usd": 512.34
    }
}
```

## Response Parameters

| Field | Type    | Description                           |
| ----- | ------- | ------------------------------------- |
| ts    | integer | Timestamp of the returned rates       |
| rates | object  | Map of fiat currency codes to rates   |
| error | string  | Error text when a rate is unavailable |

## Use Cases

* **Fiat Display**: Show a BCH amount in a user's local currency
* **Historical Valuation**: Read the rate at a past timestamp or block
* **Invoicing**: Convert a fiat-priced invoice into BCH at the current rate
* **Reporting**: Attach fiat values to transaction histories

## Error Handling

| Error Code   | Message        | Description                                                 |
| ------------ | -------------- | ----------------------------------------------------------- |
| 400 / -32602 | Bad request    | A query parameter has an invalid value                      |
| 404 / -32603 | Not found      | No rate data exists for the requested currency or timestamp |
| 500 / -32603 | Internal error | The indexer failed to read rate data                        |
