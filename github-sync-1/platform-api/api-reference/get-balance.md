---
description: >-
  Get your GetBlock CU balance and prepaid USD credits. Complete guide to GET
  /balance in the Platform API.
---

# Get balance - Platform API

`GET /balance` returns your [compute unit (CU)](../../getting-started/plans-and-limits/what-counts-as-a-cu.md) balance and your prepaid credits balance.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/balance
```
{% endcode %}

## Parameters

None.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/balance \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/balance', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.cu.total_balance);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/balance",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["cu"]["total_balance"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "cu": {
    "total_balance": 845000000,
    "main_balance": 745000000,
    "extra_balance": 100000000
  },
  "credits": {
    "balance_cents": 10000
  }
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `cu.total_balance` | integer | CU available. Equals `main_balance + extra_balance`. |
| `cu.main_balance` | integer | CU from the plan's allowance. |
| `cu.extra_balance` | integer | CU from [top-ups](../../getting-started/plans-and-limits/top-up-cus-and-boost-limits.md). |
| `credits.balance_cents` | integer | Prepaid USD balance, in cents. `10000` is $100.00. |

## Use Cases

* Alert before the CU balance runs out
* Show remaining CU on an internal dashboard
* Check prepaid credits before a top-up

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | The balance service did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
