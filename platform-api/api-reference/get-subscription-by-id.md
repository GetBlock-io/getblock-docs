---
description: >-
  Get one of your GetBlock subscriptions by id. Complete guide to GET
  /subscriptions/{id} in the Platform API.
---

# Get subscription - Platform API

`GET /subscriptions/{id}` returns one of your subscriptions. The response has the same shape as one element of [`GET /subscriptions`](list-subscriptions.md).

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/subscriptions/{id}
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The subscription's `id`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/subscriptions/s-plan \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const subscriptionId = 's-plan';

const response = await fetch(`https://public-api.getblock.io/api/v1/subscriptions/${subscriptionId}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.paid_until);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

subscription_id = "s-plan"

body = requests.get(
    f"https://public-api.getblock.io/api/v1/subscriptions/{subscription_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(body["paid_until"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "s-plan",
  "product_type": "plan",
  "product_id": "pro",
  "status": "active",
  "period": "1m",
  "type": "regular",
  "started_at": "2026-09-01T00:00:00Z",
  "paid_until": "2026-11-01T00:00:00Z",
  "current_period_end": "2026-11-01T00:00:00Z"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The subscription record, unique per subscription. |
| `product_type` | string | `plan`, `enterprise_plan`, or `request_package`. |
| `product_id` | string | The product, such as `pro` for a plan. |
| `status` | string | `active` and `grace_period` are serving; `cancelled` and `awaiting_render` are not. |
| `period` | string | Billing period, such as `1m` or `12m`. |
| `type` | string | `regular` or `trial`. |
| `started_at`, `paid_until`, `current_period_end` | string | RFC 3339 UTC times. |

## Use Cases

* Check the status and renewal date of one subscription

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 404 | No such subscription among yours. A node subscription answers `404` here too: it belongs to `/dedicated` or `/limitless`. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
