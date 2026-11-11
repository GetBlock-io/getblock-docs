---
description: >-
  The webhook and address list objects, pagination, request rules, and limits
  shared by every endpoint of the GetBlock Webhooks API.
---

# Objects and Conventions

These objects and rules apply to every endpoint of the [Webhooks API](./). Each endpoint page links here instead of repeating them.

### The webhook object

Every endpoint that returns a webhook returns this object. The signing secret is never part of it: only [Create webhook](create-webhook.md) and [Rotate webhook secret](rotate-webhook-secret.md) return a secret.

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | `wh_` followed by 12 characters. |
| `name` | string or null | Display name, at most 128 bytes. Not unique, and not sent with deliveries. |
| `status` | string | `active`, `paused`, `paused_auto`, `paused_plan`, or `paused_no_balance`. See [Webhook statuses](../pricing-and-limits.md#webhook-statuses). |
| `chain` | string | `eth`. |
| `network` | string | `mainnet`. |
| `trigger_type` | string | `address_activity`, `log_event`, or `tx_confirmation`. See [Triggers](../triggers-and-filters.md#triggers). |
| `filters` | object | The filter tree. See [Filters](../triggers-and-filters.md#filters). |
| `confirm_depth` | integer or null | Blocks to wait before the `confirmed` delivery. `null` means the chain default, 96. |
| `phases` | array of strings | `unconfirmed`, `confirmed`, or both. |
| `target_url` | string | Your endpoint, in canonical form: lower-case scheme and host, international host names in punycode. |
| `endpoint_hash` | string | Hex SHA-256 of the target's host and path. Webhooks with the same value share one endpoint for [concurrency and the circuit breaker](../retries-and-endpoint-protection.md#endpoint-protection). |
| `batching` | object | `max_events`, `max_wait_ms`, `gzip`. Stored, but has no effect in this version: every delivery carries one event. |
| `version` | integer | Grows by one on every change, including pause, resume, and secret rotation. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

### The address list object

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | `al_` followed by 12 characters. |
| `name` | string | Unique per account, at most 128 bytes. |
| `entry_count` | integer | How many addresses the list holds. |
| `version` | integer | The list's version number. Compare it to detect changes. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

A list's addresses are not part of the object: read them with [Get address list entries](get-address-list-entries.md).

### Pagination

List endpoints return one page at a time:

{% code overflow="wrap" %}
```json
{ "items": [ … ], "next_cursor": "…" }
```
{% endcode %}

| Parameter | Type | Description |
| --- | --- | --- |
| `limit` | integer | Page size. Default 50, at most 200; a larger value is treated as 200. The delivery history defaults to 100, at most 999. |
| `cursor` | string | The `next_cursor` of the previous page. A cursor the endpoint did not issue answers `400 invalid_cursor`. |

The last page has no `next_cursor`.

### Strict request bodies

Request bodies are JSON and are checked strictly: an unknown field is refused with `400 malformed_body` rather than ignored, so a misspelt option is never dropped silently. Addresses may be sent in any case, EIP-55 checksummed included, and are always returned in lower case.

### Ownership

A webhook or a list that belongs to someone else answers `404`, exactly like an unknown or malformed id. The API never reveals whether another account's id exists.


### Limits

| Limit | Value |
| --- | --- |
| Active webhooks, addresses per webhook, addresses per account, events per second | Set by your plan. See [Plan limits](../pricing-and-limits.md#plan-limits) or call [`GET /webhooks/limits`](get-webhook-limits.md). |
| Addresses written into one webhook's filter | 1,000 |
| Address lists per webhook | 10 |
| Address lists per account | 50 |
| Addresses per list | 100,000 |
| Filter tree | 8 levels deep, 256 nodes |
| Request body | About 5 MiB. 100,000 addresses (about 4.6 MB) fit in one request. |

A request over a plan limit is refused with `409` and the `cap_*` code of the limit reached.
