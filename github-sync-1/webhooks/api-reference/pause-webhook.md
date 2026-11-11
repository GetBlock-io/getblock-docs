---
description: >-
  Stop a webhook's deliveries until you resume it. Complete guide on how to use
  POST /webhooks/{id}/pause in GetBlock Webhooks documentation.
---

# Pause webhook - Webhooks

`POST /webhooks/{id}/pause` stops a webhook's deliveries until you [resume](resume-webhook.md) it. The webhook keeps its configuration and its plan slot.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/webhooks/{id}/pause
```
{% endcode %}

{% hint style="warning" %}
**Events that happen while a webhook is paused are lost.** They are not matched, not queued, and never delivered after you resume. Queued events are dropped too.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The webhook's id (`wh_…`). |

The request has no body.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/pause \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}/pause`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const webhook = await response.json();

console.log(webhook.status); // "paused"
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

webhook = requests.post(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/pause",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(webhook["status"])  # "paused"
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "wh_4Qk7Xz9pLm2R",
  "name": "usdt-transfers",
  "status": "paused",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "log_event",
  "filters": { "…": "…" },
  "confirm_depth": null,
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 3,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T11:00:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the [webhook object](objects-and-conventions.md#the-webhook-object). The fields that change:

| Field | Type | Description |
| --- | --- | --- |
| `status` | string | `paused`. |
| `version` | integer | One higher than before. A webhook you had already paused is returned as is, without a new `version`. |
| `updated_at` | string | When the webhook was paused. |

## Behavior

* Matching stops at once. A confirmed-phase delivery of an event matched **before** the pause may still arrive for up to about 60 seconds.
* Pausing a webhook that GetBlock paused (`paused_auto`, `paused_plan`, `paused_no_balance`) turns it into your own `paused`. An upgrade or a top-up then no longer applies to it: only you can resume it.
* A paused webhook still counts towards your active-webhook and address limits. [Delete](delete-webhook.md) it to free them.

## Use Cases

* Stop deliveries during maintenance of your endpoint, when losing events for that time is acceptable
* Stop spending CU on a webhook you no longer need right now, without losing its configuration

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `not_found` | No such webhook among yours. |
