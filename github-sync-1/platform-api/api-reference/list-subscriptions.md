---
description: >-
  List your GetBlock subscriptions: plans, enterprise plans, and request
  packages. Complete guide to GET /subscriptions in the Platform API.
---

# List subscriptions - Platform API

`GET /subscriptions` returns your current subscriptions: the plan and any request packages. Each is its own record, told apart by `product_type`.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/subscriptions
```
{% endcode %}

{% hint style="info" %}
This lists current commitments, not history: expired subscriptions are not returned. Dedicated and Limitless Nodes are not listed here; use [`GET /dedicated`](list-dedicated-nodes.md) and [`GET /limitless`](list-limitless-nodes.md). For the plan's limits, use [`GET /subscription`](get-subscription.md).
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Query parameter. Page size. Default 20, at most 100. |
| `offset` | integer | No | Query parameter. Items to skip. Default 0. |
| `product_type` | string | No | Query parameter. `plan`, `enterprise_plan`, or `request_package`. |
| `status` | string | No | Query parameter. Such as `active`. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl 'https://public-api.getblock.io/api/v1/subscriptions?status=active' \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/subscriptions?status=active', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

console.log(body.subscriptions.map((s) => `${s.product_type}: ${s.product_id}`));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

body = requests.get(
    "https://public-api.getblock.io/api/v1/subscriptions",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"status": "active"},
).json()

print([(s["product_type"], s["product_id"]) for s in body["subscriptions"]])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "total": 2,
  "limit": 20,
  "offset": 0,
  "subscriptions": [
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
    },
    {
      "id": "s-package",
      "product_type": "request_package",
      "product_id": "cu-100m",
      "status": "active",
      "period": "1m",
      "type": "regular",
      "started_at": "2026-10-03T12:00:00Z",
      "paid_until": "2026-11-01T00:00:00Z",
      "current_period_end": "2026-11-01T00:00:00Z"
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `total` | integer | Subscriptions matching the filters, before `limit` and `offset`. |
| `limit` | integer | Page size applied. |
| `offset` | integer | Items skipped. |
| `subscriptions` | array of objects | The subscriptions on this page. See [The subscription object](objects-and-conventions.md#the-subscription-object). |

## Use Cases

* List request packages bought on top of the plan
* Check renewal dates across all subscriptions

## Error Handling

| HTTP | Cause |
| --- | --- |
| 401 | Missing or invalid API key. |
| 502 | A GetBlock service behind the API did not answer. Retry later. |

Error bodies carry an `error` message and a `request_id`. See [Errors](objects-and-conventions.md#errors).
