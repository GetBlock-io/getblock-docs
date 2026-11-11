---
description: >-
  Get what each GetBlock plan costs your account, with introductory discounts
  applied. Complete guide to GET /pricing in the Platform API.
---

# Get pricing - Platform API

`GET /pricing` returns the plans on sale, cheapest first, each with its price per billing period and its limits.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/pricing
```
{% endcode %}

{% hint style="info" %}
**The prices are the ones your account would pay at checkout.** Discounts depend on the account, so they can differ from the [public pricing page](https://getblock.io/pricing/). Prices are whole units of `currency` (`499` is $499) for the whole period, not per month.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `search` | string | No | Query parameter. Case-insensitive substring of the plan id or name. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/pricing?search=pro' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/pricing?search=pro', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.plans.map((p) => `${p.id}: ${p.periods[0].renewal} ${p.currency}`));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/pricing",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"search": "pro"},
).json()

print([(p["id"], p["periods"][0]["renewal"], p["currency"]) for p in body["plans"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "total": 1,
  "limit": 20,
  "offset": 0,
  "plans": [
    {
      "id": "pro",
      "name": "Pro",
      "currency": "USD",
      "limits": {
        "rps": 800,
        "cu_amount": 1000000000,
        "max_tokens": 150,
        "cu_replenishment": "monthly"
      },
      "periods": [
        {
          "period": "1m",
          "base": 499,
          "first_payment": 499,
          "renewal": 499
        },
        {
          "period": "12m",
          "base": 5988,
          "first_payment": 4790,
          "renewal": 4790
        }
      ]
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Plans matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `plans[].id` | string | Plan id. The same id [`GET /subscription`](get-subscription.md) reports. |
| `plans[].name` | string | Display name. |
| `plans[].currency` | string | Currency of every price in the entry, such as `USD`. |
| `plans[].limits` | object | `rps`, `cu_amount`, `max_tokens`, and `cu_replenishment`, as in [`GET /subscription`](get-subscription.md). |
| `plans[].periods[].period` | string | Billing period, such as `1m` or `12m`. Shortest first. |
| `plans[].periods[].base` | integer | The undiscounted price for the period. |
| `plans[].periods[].first_payment` | integer | What a new customer pays for the first period. |
| `plans[].periods[].renewal` | integer | What is paid afterwards. Differs from `first_payment` only when an introductory discount applies. |

Plans no longer sold are left out. [`GET /subscription`](get-subscription.md) can still name one, because an account may still be on it.

## Use Cases

* Compare plans by price per CU or per RPS
* Show the upgrade price in an internal billing tool

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
