---
description: >-
  List your webhooks, newest first, one page at a time. Complete guide on how
  to use GET /webhooks in GetBlock Webhooks documentation.
---

# List webhooks - Webhooks

`GET /webhooks` returns your webhooks, newest first, one page at a time. Deleted webhooks are not listed, and the signing secret is never included.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks
```
{% endcode %}

## Parameters

Query parameters:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `limit` | integer | No | Page size. Default 50, at most 200; a larger value is treated as 200. |
| `cursor` | string | No | The `next_cursor` of the previous page. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl "https://public-api.getblock.io/api/v1/webhooks?limit=50" \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
// Collect every page
const webhooks = [];
let cursor;

do {
  const url = new URL('https://public-api.getblock.io/api/v1/webhooks');
  url.searchParams.set('limit', '200');
  if (cursor) url.searchParams.set('cursor', cursor);

  const response = await fetch(url, {
    headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
  });
  const page = await response.json();

  webhooks.push(...page.items);
  cursor = page.next_cursor;
} while (cursor);

console.log(webhooks.map((w) => `${w.id} ${w.status}`));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

headers = {"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"}
params = {"limit": 200}
webhooks = []

while True:  # collect every page
    page = requests.get(
        "https://public-api.getblock.io/api/v1/webhooks", headers=headers, params=params
    ).json()
    webhooks.extend(page["items"])
    if "next_cursor" not in page:
        break
    params["cursor"] = page["next_cursor"]

for w in webhooks:
    print(w["id"], w["status"])
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
      "id": "wh_4Qk7Xz9pLm2R",
      "name": "usdt-transfers",
      "status": "active",
      "chain": "eth",
      "network": "mainnet",
      "trigger_type": "log_event",
      "filters": { "all": [ { "type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"] }, { "…": "…" } ] },
      "confirm_depth": null,
      "phases": ["unconfirmed", "confirmed"],
      "target_url": "https://hooks.example.io/getblock",
      "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
      "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
      "version": 1,
      "created_at": "2026-09-25T10:00:00Z",
      "updated_at": "2026-09-25T10:00:00Z"
    }
  ]
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `items` | array | One page of [webhook objects](objects-and-conventions.md#the-webhook-object), newest first. |
| `next_cursor` | string | Pass it as `cursor` for the next page. Absent on the last page. |

## Use Cases

* Show all webhooks of an account in your own admin panel
* Find webhooks in `paused_auto` or `paused_no_balance` so you can [resume](resume-webhook.md) them
* Audit which endpoints receive your events

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `invalid_limit` | `limit` is not an integer. |
| 400 | `invalid_cursor` | `cursor` was not issued by this endpoint. |
