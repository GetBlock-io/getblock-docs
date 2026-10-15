---
description: >-
  Get your current GetBlock plan and its limits: RPS, CU allowance, token cap,
  and CU replenishment. Complete guide to GET /subscription in the Platform
  API.
---

# Get current plan - Platform API

`GET /subscription` returns the plan you are on and its limits. To list add-ons and request packages as well, use [`GET /subscriptions`](list-subscriptions.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/subscription
```
{% endcode %}

## Parameters

None.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/subscription \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/subscription', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.plan?.limits);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/subscription",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["plan"]["limits"] if body["has_plan"] else None)
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "has_plan": true,
  "plan": {
    "id": "pro",
    "name": "Pro",
    "status": "active",
    "period": "1m",
    "type": "regular",
    "limits": {
      "rps": 800,
      "cu_amount": 1000000000,
      "max_tokens": 150,
      "cu_replenishment": "monthly"
    },
    "started_at": "2025-09-13T10:20:06Z",
    "paid_until": "2026-11-01T00:00:00Z",
    "current_period_end": "2026-11-01T00:00:00Z"
  }
}
```
{% endcode %}

An account without a plan is a normal state, not an error. It answers `200` with `{"has_plan": false, "plan": null}`.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `has_plan` | boolean | Whether you are on a plan. |
| `plan` | object or null | `null` when `has_plan` is `false`. |
| `plan.id` | string | Plan id, such as `pro`. The same id [`GET /pricing`](get-pricing.md) uses. |
| `plan.name` | string | Display name. Empty for plans that are not publicly listed; use `id` instead. |
| `plan.status` | string | `active` or `grace_period`. A plan in `grace_period` still serves requests: it is a payment state, not a suspension. |
| `plan.period` | string | Billing period, such as `1m`. |
| `plan.type` | string | `regular` or `trial`. |
| `plan.limits.rps` | integer | Requests per second. |
| `plan.limits.cu_amount` | integer | Compute units granted per replenishment period. |
| `plan.limits.max_tokens` | integer | Maximum number of access tokens. |
| `plan.limits.cu_replenishment` | string | How often the CU quota resets, such as `monthly`. |
| `plan.started_at`, `plan.paid_until`, `plan.current_period_end` | string | RFC 3339 UTC times. |

A limit of `0` means the value is not reported for the plan, not a zero allowance.

## Use Cases

* Check `max_tokens` before creating tokens in bulk
* Warn when `paid_until` is close
* Configure client-side rate limiting from `rps`

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
