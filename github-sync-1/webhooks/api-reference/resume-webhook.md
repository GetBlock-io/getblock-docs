---
description: >-
  Make a paused webhook active again. Complete guide on how to use POST
  /webhooks/{id}/resume in GetBlock Webhooks documentation.
---

# Resume webhook - Webhooks

`POST /webhooks/{id}/resume` makes a paused webhook `active` again. It works for every paused status: `paused`, `paused_auto`, `paused_plan`, and `paused_no_balance`.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/webhooks/{id}/resume
```
{% endcode %}

{% hint style="info" %}
**Nothing resumes a webhook automatically**, except an upgrade for a `paused_plan` webhook. A webhook paused after failures (`paused_auto`) or an empty balance (`paused_no_balance`) stays paused until you call this endpoint or click **Resume** in the dashboard.
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
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/resume \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}/resume`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const body = await response.json();

if (response.status === 409 && body.error === 'insufficient_balance') {
  console.log('Top up your CU balance, then resume again');
} else {
  console.log(body.status); // "active"
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

response = requests.post(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/resume",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
)
body = response.json()

if response.status_code == 409 and body["error"] == "insufficient_balance":
    print("Top up your CU balance, then resume again")
else:
    print(body["status"])  # "active"
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
  "status": "active",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "log_event",
  "filters": { "…": "…" },
  "confirm_depth": null,
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 4,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T12:00:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the [webhook object](objects-and-conventions.md#the-webhook-object). The fields that change:

| Field | Type | Description |
| --- | --- | --- |
| `status` | string | `active`. |
| `version` | integer | One higher than before. |
| `updated_at` | string | When the webhook was resumed. |

## Before you resume

| Status | Fix first |
| --- | --- |
| `paused` | Nothing. |
| `paused_auto` | Make your endpoint answer `2xx` within 15 seconds. Check [`GET /webhooks/{id}/deliveries`](get-webhook-deliveries.md) for the cause. |
| `paused_no_balance` | Top up your CU balance. A resume in the one to two minutes after a top-up is accepted; deliveries start once the balance clears. |
| `paused_plan` | Upgrade, or reduce your webhooks and addresses until this one fits your plan. |

Events from the time the webhook was paused are not delivered after it resumes. See [When the balance runs out](../pricing-and-limits.md#when-the-balance-runs-out).

## Use Cases

* Bring a webhook back after fixing a failing endpoint
* Restart deliveries after a CU top-up
* Automate recovery: poll [List webhooks](list-webhooks.md) for paused statuses and resume once the cause is fixed

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 403 | `plan_required` | A team account without a plan, resuming a `paused_plan` webhook. |
| 404 | `not_found` | No such webhook among yours. |
| 409 | `insufficient_balance` | A `paused_no_balance` webhook while the balance is still empty. Nothing changes. |
| 409 | `cap_active_webhooks_exceeded`, `cap_addresses_exceeded`, `cap_account_addresses_exceeded` | The webhook no longer fits your plan. |
