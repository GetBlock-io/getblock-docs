---
description: >-
  Get one of your webhooks by id. Complete guide on how to use GET
  /webhooks/{id} in GetBlock Webhooks documentation.
---

# Get webhook - Webhooks

`GET /webhooks/{id}` returns one of your webhooks. The signing secret is never included.

{% code overflow="wrap" %}
```
GET https://public-api.getblock.io/api/v1/webhooks/{id}
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
curl https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}`, {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});
const webhook = await response.json();

console.log(webhook.status);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

webhook = requests.get(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
).json()

print(webhook["status"])
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
  "id": "wh_4Qk7Xz9pLm2R",
  "name": "treasury-wallets",
  "status": "active",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "address_activity",
  "filters": {
    "type": "address",
    "dir": "any",
    "list_refs": ["al_2bQ4rTz9KmNp"],
    "src": "sugar"
  },
  "confirm_depth": null,
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 1,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:00:00Z"
}
```
{% endcode %}

## Response Parameters

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | `wh_` followed by 12 characters. |
| `name` | string or null | Display name, at most 128 bytes. Not unique, and not sent with deliveries. |
| `status` | string | `active`, `paused`, `paused_auto`, `paused_plan`, or `paused_no_balance`. See [Webhook statuses](../pricing-and-limits.md#webhook-statuses). |
| `chain` | string | `eth`. |
| `network` | string | `mainnet`. |
| `trigger_type` | string | `address_activity`, `log_event`, or `tx_confirmation`. |
| `filters` | object | The filter tree. A leaf with `"src": "sugar"` was built from `addresses` or `list_refs`. |
| `confirm_depth` | integer or null | Blocks before the `confirmed` delivery. `null` means the chain default, 96. |
| `phases` | array of strings | `unconfirmed`, `confirmed`, or both. |
| `target_url` | string | Your endpoint, in canonical form. |
| `endpoint_hash` | string | Hex SHA-256 of the target's host and path. Webhooks with the same value share one endpoint for [concurrency and the circuit breaker](../retries-and-endpoint-protection.md#endpoint-protection). |
| `batching` | object | Stored, but has no effect in this version. |
| `version` | integer | Grows by one on every change, including pause, resume, and secret rotation. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

{% hint style="warning" %}
Before you send `filters` back in a [`PATCH`](update-webhook.md), remove any leaf that carries `"src": "sugar"`. The marker is reserved, and a tree that contains it is refused with `400 filter_reserved_field`.
{% endhint %}

## Use Cases

* Check a webhook's `status` after a balance top-up or an endpoint outage
* Read the current filter before you change it
* Confirm a change took effect by comparing `version`

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 404 | `not_found` | No such webhook among yours. A deleted webhook, another account's webhook, and a malformed id answer the same way. |
