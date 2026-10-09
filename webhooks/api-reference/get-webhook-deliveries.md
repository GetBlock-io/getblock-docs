---
description: >-
  List a webhook's failed delivery attempts, newest first. Complete guide on
  how to use GET /webhooks/{id}/deliveries in GetBlock Webhooks documentation.
---

# Get webhook deliveries - Webhooks

`GET /webhooks/{id}/deliveries` returns the webhook's delivery attempts that did not succeed (retries and final failures), newest first. It shows the same data as the **Deliveries** tab of the webhook in the dashboard.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks/{id}/deliveries
```
{% endcode %}

{% hint style="info" %}
**Successful attempts are not listed**, except test events. Use [`GET /webhooks/{id}/stats`](get-webhook-stats.md) to count what got through. Under a long failure the log keeps only a sample, about one row a second after the first 20; the exact count is `failed_total` in the stats.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The webhook's id (`wh_…`). |

Query parameters:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `from` | string | No | RFC 3339 time. Only attempts at or after it. |
| `to` | string | No | RFC 3339 time. Only attempts before it. |
| `status` | string | No | `retrying` (will be retried) or `failed_terminal` (given up). Repeatable or comma-separated. `delivered` and `replayed` are accepted, but only test events are ever `delivered` and nothing is replayed. |
| `limit` | integer | No | Page size. Default 100, at most 999. |
| `cursor` | string | No | The `next_cursor` of the previous page. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl "https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/deliveries?status=failed_terminal&from=2026-09-25T00:00:00Z" \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';
const url = new URL(`https://public-api.getblock.io/api/v1/webhooks/${id}/deliveries`);
url.searchParams.set('status', 'failed_terminal');
url.searchParams.set('from', '2026-09-25T00:00:00Z');

const response = await fetch(url, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const { items } = await response.json();

for (const a of items) {
  console.log(a.delivered_at, a.attempt, a.http_status ?? a.error);
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

page = requests.get(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}/deliveries",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    params={"status": "failed_terminal", "from": "2026-09-25T00:00:00Z"},
).json()

for a in page["items"]:
    print(a["delivered_at"], a["attempt"], a["http_status"] or a.get("error"))
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "items": [
    {
      "event_id": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0",
      "attempt": 2,
      "status": "retrying",
      "http_status": 503,
      "latency_ms": 1200,
      "delivered_at": "2026-09-25T10:00:20Z"
    }
  ],
  "next_cursor": "…"
}
```
{% endcode %}

An empty page is `{"items": []}`.

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `items[].event_id` | string | The event's id, as your endpoint received it in `eventId`. |
| `items[].attempt` | integer | Attempt number of this event, from 1 to 8. |
| `items[].status` | string | `retrying`: will be retried. `failed_terminal`: given up after the last attempt. `delivered`: test events only. |
| `items[].http_status` | integer or null | Your endpoint's HTTP status. `null` when no response was received. |
| `items[].latency_ms` | integer or null | How long the attempt took. |
| `items[].error` | string | Why the attempt failed, for example a DNS, connection, or TLS error. Omitted when there is nothing to add. |
| `items[].delivered_at` | string | When the attempt was made. |
| `next_cursor` | string | Pass it as `cursor` for the next page. Absent on the last page. |

The retry schedule and what counts as a failure are in [Retries and Endpoint Protection](../retries-and-endpoint-protection.md).

## Use Cases

* Find out why a webhook became `paused_auto` before you [resume](resume-webhook.md) it
* Get the `event_id`s of events that were given up, so you can fetch that data from the chain yourself (there is no replay)
* Spot slow answers close to the 15-second timeout from `latency_ms`

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `invalid_from`, `invalid_to` | Not an RFC 3339 time. |
| 400 | `invalid_time_range` | `from` is not before `to`. |
| 400 | `invalid_status` | An unknown `status` value. |
| 400 | `invalid_limit`, `invalid_cursor` | `limit` is not an integer, or `cursor` was not issued by this endpoint. |
| 404 | `not_found` | No such webhook among yours. |
| 503 | `delivery_log_not_configured` | The delivery log is temporarily unavailable. Retry later. |
