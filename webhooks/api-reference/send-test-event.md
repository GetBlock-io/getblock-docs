---
description: >-
  Send a free, signed test event to a webhook's endpoint. Complete guide on how
  to use POST /webhooks/{id}/test in GetBlock Webhooks documentation.
---

# Send test event - Webhooks

`POST /webhooks/{id}/test` queues one test event for the webhook. The event has the shape of the webhook's live events with sample mainnet data, and it is signed and sent to `target_url` like any other delivery.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/webhooks/{id}/test
```
{% endcode %}

{% hint style="info" %}
Test events are **free** and are sent even at a zero balance. They still count like any other delivery in the webhook's stats, and repeated failures bring an [automatic pause](../retries-and-endpoint-protection.md#endpoint-protection) closer.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | Path parameter. The webhook's id (`wh_…`). The webhook must be `active`. |

The request has no body.

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/test \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}/test`, {
  method: 'POST',
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const { event_id, status } = await response.json();

console.log(event_id, status); // look for this eventId in your endpoint's logs
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

test = requests.post(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/test",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(test["event_id"], test["status"])  # look for this eventId in your endpoint's logs
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

`202 Accepted`:

{% code overflow="wrap" %}
```json
{
  "event_id": "…:test:…",
  "status": "queued"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `event_id` | string | The test event's id, as your endpoint receives it in `eventId`. Test event ids contain `:test:`. |
| `status` | string | `queued`: accepted, not yet delivered. |

`202` does not mean your endpoint received the event. The outcome shows in [`GET /webhooks/{id}/stats`](get-webhook-stats.md) and, for a failure, in [`GET /webhooks/{id}/deliveries`](get-webhook-deliveries.md). Each call is a new event with its own `event_id`. See [Test deliveries](../delivery-format.md#test-deliveries) for the payload.

## Use Cases

* Check that a new endpoint receives deliveries and [verifies signatures](../verifying-signatures.md) before live events arrive
* Confirm a new `target_url` or a rotated secret works
* Exercise your handler in a staging environment without waiting for on-chain activity

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `not_found` | No such webhook among yours. |
| 409 | `webhook_not_active` | The webhook is paused. [Resume](resume-webhook.md) it first. |
| 503 | `test_event_unavailable` | The event could not be queued in time. It may still be delivered; a retry sends a second, separate test event. |
