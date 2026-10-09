---
description: >-
  Change some of a webhook's settings: endpoint, phases, filter, addresses or
  name. Complete guide on how to use PATCH /webhooks/{id} in GetBlock Webhooks
  documentation.
---

# Update webhook - Webhooks

`PATCH /webhooks/{id}` changes only the fields you send and leaves the rest as they are. The same plan limits as on create apply to the result, and `version` grows by one.

{% code overflow="wrap" %}
```
PATCH https://public-api.getblock.io/api/v1/webhooks/{id}
```
{% endcode %}

{% hint style="warning" %}
`chain`, `network`, and `trigger_type` cannot be changed: sending them is refused with `400 malformed_body`. Create a new webhook instead.
{% endhint %}

## Parameters

Path parameter:

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | Yes | The webhook's id (`wh_…`). |

Body fields. Every field is optional, but the body must not be empty:

| Parameter | Type | Description |
| --- | --- | --- |
| `name` | string or null | A string renames the webhook; `null` removes the name. |
| `target_url` | string | A new endpoint. Checked against the [target URL rules](../retries-and-endpoint-protection.md#target-url-rules). |
| `phases` | array of strings | `["unconfirmed"]`, `["confirmed"]`, or both. |
| `confirm_depth` | integer or null | 2 to 96 sets the depth; `null` resets it to the default, 96. Leaving it out keeps the current value. |
| `filters` | object | Replaces the filter tree. Remove any leaf with `"src": "sugar"` first. |
| `addresses` | array of strings | **Replaces** the webhook's own addresses; `[]` clears them. |
| `list_refs` | array of strings | **Replaces** the referenced address lists; `[]` clears them. |
| `batching` | object | Stored, but has no effect in this version. |

A change that would leave the webhook with no condition at all is refused with `400 filter_would_be_empty`: send `filters` in the same request. See [Changing a webhook](../triggers-and-filters.md#changing-a-webhook).

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X PATCH https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"target_url": "https://hooks.example.io/getblock-v2", "phases": ["confirmed"]}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const id = 'wh_4Qk7Xz9pLm2R';

const response = await fetch(`https://public-api.getblock.io/api/v1/webhooks/${id}`, {
  method: 'PATCH',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    target_url: 'https://hooks.example.io/getblock-v2',
    phases: ['confirmed']
  })
});

console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

webhook_id = "wh_4Qk7Xz9pLm2R"

response = requests.patch(
    f"https://public-api.getblock.io/api/v1/webhooks/{webhook_id}",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={"target_url": "https://hooks.example.io/getblock-v2", "phases": ["confirmed"]},
)

print(response.json())
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
  "filters": { "all": [ { "type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"] }, { "…": "…" } ] },
  "confirm_depth": null,
  "phases": ["confirmed"],
  "target_url": "https://hooks.example.io/getblock-v2",
  "endpoint_hash": "…",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 2,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:05:00Z"
}
```
{% endcode %}

## Response Parameters

The answer is the updated [webhook object](./#the-webhook-object). The fields to check after an update:

| Field | Type | Description |
| --- | --- | --- |
| `version` | integer | One higher than before the change. |
| `target_url` | string | The new URL in canonical form. |
| `endpoint_hash` | string | Changes when the host or path of `target_url` changes. |
| `updated_at` | string | When the change was made. |

The change applies to the next event at once. Retries that are already scheduled may use the previous settings for up to 60 seconds.

## Use Cases

* Move deliveries to a new endpoint URL
* Switch from both phases to `confirmed` only to halve the CU per event
* Replace the watched addresses after a wallet rotation
* Raise or lower the confirmation depth

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body` | The body is empty. |
| 400 | `malformed_body` | Invalid JSON, an unknown field, a wrong type, or `chain`, `network`, or `trigger_type` in the body. |
| 400 | `invalid_webhook_name`, `invalid_phases`, `confirm_depth_out_of_range`, `invalid_batching` | A field has an invalid value. |
| 400 | `filter_would_be_empty` | The change would leave the webhook with no condition. |
| 400 | `filter_reserved_field` | `filters` still contains a leaf with `"src": "sugar"`. |
| 400 | `target_url_*`, other `filter_*` codes | See [Error codes](./#error-codes). |
| 404 | `not_found` | No such webhook among yours. |
| 404 | `list_not_found` | A `list_refs` entry is not one of your address lists. |
| 409 | `cap_addresses_exceeded`, `cap_inline_addresses_exceeded`, `cap_list_refs_exceeded`, `cap_account_addresses_exceeded` | The result does not fit your plan. |
| 413 | — | The body is larger than about 5 MiB. |
