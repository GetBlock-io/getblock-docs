---
description: >-
  Get a webhook's delivery totals and recent delivery rates. Complete guide on
  how to use GET /webhooks/{id}/stats in GetBlock Webhooks documentation.
---

# Get webhook stats - Webhooks

`GET /webhooks/{id}/stats` returns the webhook's exact delivery totals and, when available, its delivery rates over the last minute, 5 minutes, and hour. It shows the same data as the **Stats** tab of the webhook in the dashboard.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks/{id}/stats
```
{% endcode %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The webhook's id (`wh_…`). |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/stats \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}/stats`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const stats = await response.json();

if (stats.rates_available) {
  const { delivered, failed } = stats.rates.window_300s;
  console.log(`Last 5 min: ${delivered} delivered, ${failed} failed`);
}
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

stats = requests.get(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/stats",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

if stats["rates_available"]:
    window = stats["rates"]["window_300s"]
    print(f"Last 5 min: {window['delivered']} delivered, {window['failed']} failed")
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "webhook_id": "wh_4Qk7Xz9pLm2R",
  "delivered_total": 1520,
  "failed_total": 4,
  "rate_limited_total": 0,
  "last_delivered_at": "2026-09-25T10:04:58Z",
  "updated_at": "2026-09-25T10:05:00Z",
  "rates": {
    "window_60s": { "delivered": 3, "failed": 0 },
    "window_300s": { "delivered": 14, "failed": 1 },
    "window_3600s": { "delivered": 161, "failed": 4 }
  },
  "rates_available": true
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `webhook_id` | string | The webhook's id. |
| `delivered_total` | integer | Successful attempts, test deliveries included. |
| `failed_total` | integer | Failed attempts, each retry counted, test deliveries included. Also counts attempts spent without a request while the [circuit breaker](../retries-and-endpoint-protection.md#endpoint-protection) is open. |
| `rate_limited_total` | integer | Events dropped above your plan's events-per-second limit. They were not attempted and not billed. |
| `last_delivered_at` | string or null | Time of the last successful delivery. `null` if there has never been one. |
| `updated_at` | string or null | When the totals were last updated. |
| `rates` | object or null | Delivered and failed counts over the last minute (`window_60s`), 5 minutes (`window_300s`), and hour (`window_3600s`). `null` when `rates_available` is `false`. |
| `rates_available` | boolean | `false` when the short-window rates are temporarily unavailable. The totals are still exact. |

Totals update with a delay of a few seconds. A webhook that has never had a delivery answers zeros with `null` times.

## Use Cases

* Alert when `failed` grows in `window_300s`, before auto-pause stops the webhook
* Detect a too-small plan when `rate_limited_total` grows
* Confirm that a [test event](send-test-event.md) was delivered

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `not_found` | No such webhook among yours. |
