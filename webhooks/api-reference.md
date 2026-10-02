---
description: >-
  Reference of the GetBlock Public API for webhooks and address lists:
  authentication, pagination, every endpoint with examples, and error codes.
---

# API Reference

Everything you do with webhooks in the dashboard you can also do through the Public API. The full schema is in the interactive [Public API reference](https://public-api.getblock.io/docs/v1).

### Base URL and authentication

```
https://public-api.getblock.io/api/v1
```

Authenticate every request with an API key from [**Settings → API Keys**](https://account.getblock.io/settings/api-keys) in your GetBlock account. Keys start with `gb_`:

```
Authorization: Bearer <API-KEY>
```

The examples below read the key from the `GETBLOCK_API_KEY` environment variable.

* Request and response bodies are JSON. Request bodies are strict: an unknown field is refused with `400 malformed_body` rather than ignored, so a misspelt option is never dropped silently.
* An error answer is `{"error": "<code>", "request_id": "…"}`. `error` is a stable code to branch on — see [Error codes](#error-codes).
* A webhook or a list that belongs to someone else answers `404`, exactly like an unknown id.

### Pagination

List endpoints return one page at a time:

{% code overflow="wrap" %}
```json
{ "items": [ … ], "next_cursor": "…" }
```
{% endcode %}

Pass `next_cursor` as the `cursor` query parameter to get the next page; the last page has no `next_cursor`. `limit` sets the page size: 50 by default, at most 200 (larger values are treated as 200). The delivery history has its own default of 100 and maximum of 999.

### Webhooks

| Method and path | Description | Success |
| --- | --- | --- |
| `POST /webhooks` | Create a webhook | `201`, the webhook with its `secret` (shown once) |
| `GET /webhooks` | List your webhooks, newest first | `200`, a page of webhooks |
| `GET /webhooks/limits` | Your plan's limits and usage | `200` |
| `GET /webhooks/{id}` | Get a webhook | `200` |
| `PATCH /webhooks/{id}` | Update a webhook | `200`, the updated webhook |
| `DELETE /webhooks/{id}` | Delete a webhook | `204` |
| `POST /webhooks/{id}/pause` | Pause deliveries | `200`, the webhook |
| `POST /webhooks/{id}/resume` | Resume deliveries | `200`, the webhook |
| `POST /webhooks/{id}/secret/rotate` | Rotate the signing secret | `200`, `{"secret", "version", "previous_expires_at"}` |
| `POST /webhooks/{id}/test` | Send a test event | `202`, `{"event_id", "status": "queued"}` |
| `GET /webhooks/{id}/deliveries` | Failed delivery attempts | `200`, a page of attempts |
| `GET /webhooks/{id}/stats` | Delivery counters | `200` |
| `GET /webhooks/{id}/addresses` | The addresses given in `addresses` | `200`, `{"list_id", "addresses", "next_cursor"}` |

Paths are relative to the base URL. The secret is never part of a webhook object: only the create and rotate answers carry it.

#### The webhook object

| Field | Description |
| --- | --- |
| `id` | `wh_` followed by 12 characters |
| `name` | Display name, at most 128 bytes, or `null`. Not unique, not sent with deliveries |
| `status` | `active`, `paused`, `paused_auto`, `paused_plan` or `paused_no_balance` — see [Webhook statuses](pricing-and-limits.md#webhook-statuses) |
| `chain`, `network` | `eth`, `mainnet` |
| `trigger_type` | `address_activity`, `log_event` or `tx_confirmation` |
| `filters` | The filter tree — see [Filters](triggers-and-filters.md#filters) |
| `confirm_depth` | Blocks before the confirmed delivery, or `null` for the default |
| `phases` | `unconfirmed`, `confirmed` or both |
| `target_url` | The endpoint, in canonical form |
| `endpoint_hash` | Hex SHA-256 of the target's host and path; webhooks with the same value share one endpoint for [concurrency and the circuit breaker](retries-and-endpoint-protection.md#endpoint-protection) |
| `batching` | Stored, but has no effect in this version |
| `version` | Grows by one on every change, including pause, resume and secret rotation |
| `created_at`, `updated_at` | RFC 3339 times |

#### Create a webhook

`POST /webhooks`

| Field | Required | Description |
| --- | --- | --- |
| `chain` | Yes | `eth` |
| `network` | No | `mainnet` (the default) |
| `trigger_type` | Yes | `address_activity`, `log_event` or `tx_confirmation` |
| `phases` | Yes | `["unconfirmed"]`, `["confirmed"]` or both. Ignored by `tx_confirmation`, but still required |
| `target_url` | Yes | An `https` URL on a public domain — see [Target URL rules](retries-and-endpoint-protection.md#target-url-rules) |
| `filters` | One of these three | The filter tree |
| `addresses` | One of these three | Addresses to watch in both directions |
| `list_refs` | One of these three | Address list ids to watch in both directions |
| `confirm_depth` | No | 2 to 96; leave it out or send `null` for the default, 96 |
| `name` | No | Display name, at most 128 bytes |

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "usdt-transfers",
    "chain": "eth",
    "network": "mainnet",
    "trigger_type": "log_event",
    "phases": ["unconfirmed", "confirmed"],
    "target_url": "https://hooks.example.io/getblock",
    "filters": {
      "all": [
        { "type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"] },
        { "type": "topic", "position": 0, "in": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }
      ]
    }
  }'
```
{% endcode %}

The answer is `201` with the [webhook object](#the-webhook-object) and its `secret` (`whsec_…`). **Store the secret now** — no other answer returns it. A plan limit answers `409` with the `cap_*` code of the limit reached. More request bodies are in [Triggers and Filters](triggers-and-filters.md#examples).

#### List webhooks

`GET /webhooks?limit=50&cursor=…` returns your webhooks, newest first, without deleted ones:

{% code overflow="wrap" %}
```bash
curl https://public-api.getblock.io/api/v1/webhooks -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

#### Get your limits

`GET /webhooks/limits` returns the limits the next create or update is checked against, and your current address usage:

{% code overflow="wrap" %}
```json
{
  "tier": "pro",
  "account_type": "personal",
  "plan_required": false,
  "caps": {
    "active_webhooks": 75,
    "addresses_per_webhook": 250000,
    "inline_addresses": 1000,
    "list_refs": 10,
    "addresses_per_list": 100000,
    "address_lists": 50,
    "addresses_per_account": 1000000,
    "events_per_second": 30,
    "events_burst": 360
  },
  "usage": { "account_addresses": 3 }
}
```
{% endcode %}

* `addresses_per_account: 0` means no account-wide limit.
* `events_per_second` and `events_burst`: `0` means the plan sets none and the platform default applies.
* A team account without a plan gets `"plan_required": true`; creating a webhook or an address list then answers `403 plan_required`.

The answer is advisory: the create's own answer is authoritative. See [Plan limits](pricing-and-limits.md#plan-limits).

#### Get, update and delete

* `GET /webhooks/{id}` returns the [webhook object](#the-webhook-object).
* `PATCH /webhooks/{id}` changes only the fields you send: `name`, `filters`, `confirm_depth`, `phases`, `target_url`, `addresses`, `list_refs`, `batching`. `chain`, `network` and `trigger_type` cannot be changed (`400 malformed_body`). `addresses` and `list_refs` replace the current values and `[]` clears them; `"name": null` removes the name; `"confirm_depth": null` resets the depth to the default. See [Changing a webhook](triggers-and-filters.md#changing-a-webhook).
* `DELETE /webhooks/{id}` answers `204`. Deliveries stop and the plan slot is freed. This cannot be undone: the webhook, its stats and its delivery history are no longer reachable.

{% code overflow="wrap" %}
```bash
curl -X PATCH https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"target_url": "https://hooks.example.io/getblock-v2", "phases": ["confirmed"]}'
```
{% endcode %}

#### Pause and resume

`POST /webhooks/{id}/pause` and `POST /webhooks/{id}/resume`, without a body, return the webhook.

* While a webhook is paused, events are not matched and are never delivered later. A paused webhook keeps its plan slot.
* Pausing a webhook that GetBlock paused (`paused_auto`, `paused_plan`, `paused_no_balance`) turns it into your own `paused`.
* Resume answers `409 insufficient_balance` for a `paused_no_balance` webhook while the balance is empty, and a `409` `cap_*` code when the webhook no longer fits your plan.

#### Rotate the secret

`POST /webhooks/{id}/secret/rotate`. Without a body (or with `{"expire_previous": false}`) the previous secret keeps working for 24 hours; `{"expire_previous": true}` retires it at once.

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/secret/rotate \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

```json
{ "secret": "whsec_8Zt3kW1qP5nR7yL2vB9cX4mH6", "version": 4, "previous_expires_at": "2026-09-26T10:00:00Z" }
```

See [Rotating the secret](verifying-signatures.md#rotating-the-secret).

#### Send a test event

`POST /webhooks/{id}/test`, without a body, answers `202` with `{"event_id": "…", "status": "queued"}`: queued, not yet delivered. Free; only for an `active` webhook (`409 webhook_not_active` otherwise). See [Test deliveries](delivery-format.md#test-deliveries).

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/test \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

#### Delivery history and stats

* `GET /webhooks/{id}/deliveries?from=…&to=…&status=…&limit=…&cursor=…` — failed attempts, newest first.
* `GET /webhooks/{id}/stats` — `delivered_total`, `failed_total`, `rate_limited_total` and delivery rates.

The fields and filters are described in [Delivery history and stats](retries-and-endpoint-protection.md#delivery-history-and-stats).

{% code overflow="wrap" %}
```bash
curl "https://public-api.getblock.io/api/v1/webhooks/wh_4Qk7Xz9pLm2R/deliveries?status=failed_terminal&from=2026-09-25T00:00:00Z" \
  -H "Authorization: Bearer $GETBLOCK_API_KEY"
```
{% endcode %}

#### The webhook's own addresses

`GET /webhooks/{id}/addresses?limit=…&cursor=…` returns the addresses the webhook was given in `addresses`, in lower case, with the id of the list that holds them:

{% code overflow="wrap" %}
```json
{ "list_id": "al_2bQ4rTz9KmNp", "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"] }
```
{% endcode %}

That list belongs to the webhook: it does not appear under `/address-lists` and changes only through `PATCH /webhooks/{id}` with `addresses`. A webhook without `addresses` answers `"list_id": null` and an empty array.

### Address lists

A named address list holds addresses that several webhooks reference through `list_refs`. See [Address lists](triggers-and-filters.md#address-lists) for the limits.

| Method and path | Description | Success |
| --- | --- | --- |
| `POST /address-lists` | Create a list, optionally with its first addresses | `201`, the list |
| `GET /address-lists` | List your address lists, newest first | `200`, a page of lists |
| `GET /address-lists/{id}` | Get a list, without its addresses | `200`, the list |
| `PATCH /address-lists/{id}` | Rename a list | `200`, the list |
| `DELETE /address-lists/{id}` | Delete a list | `204` |
| `GET /address-lists/{id}/entries` | The addresses in a list | `200`, `{"addresses", "next_cursor"}` |
| `POST /address-lists/{id}/entries` | Add addresses | `200`, the list |
| `PUT /address-lists/{id}/entries` | Replace all addresses; `[]` clears the list | `200`, the list |
| `POST /address-lists/{id}/entries/remove` | Remove addresses | `200`, the list |

A list object is `{"id": "al_…", "name", "entry_count", "version", "created_at", "updated_at"}`. Names are unique per account, at most 128 bytes.

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/address-lists \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "Treasury wallets", "addresses": ["0x742d35cc6634c0532925a3b844bc454e4438f44e"]}'
```
{% endcode %}

{% code overflow="wrap" %}
```json
{
  "id": "al_7hK2mPq9Wx3Z",
  "name": "Treasury wallets",
  "entry_count": 1,
  "version": 1,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:00:00Z"
}
```
{% endcode %}

Add, replace and remove take the same body — `{"addresses": ["0x…", …]}`, up to 100,000 addresses per request:

{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/address-lists/al_7hK2mPq9Wx3Z/entries \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"addresses": ["0x2f1528f344f6410d361654d22a47ef121a60c938"]}'
```
{% endcode %}

* Adding ignores addresses already in the list; removing ignores addresses not in it.
* Growing a list is checked against the limits of every webhook that references it; one failure rejects the whole request.
* Webhooks that reference the list match the new set at once.
* `PATCH /address-lists/{id}` with `{"name": "Cold wallets"}` renames the list.
* `DELETE /address-lists/{id}` answers `409 list_in_use` while a webhook still references the list: remove it from the webhook's `list_refs`, or delete the webhook, first.

### Error codes

| HTTP | Code | When |
| --- | --- | --- |
| 400 | `malformed_body` | Invalid JSON, an unknown field, a wrong type, or `chain` / `network` / `trigger_type` in a `PATCH` |
| 400 | `empty_body` | A body is required |
| 400 | `invalid_trigger_type` | Unknown `trigger_type` |
| 400 | `trigger_type_not_supported` | `dropped_replaced` is not available |
| 400 | `invalid_phases` | `phases` empty or unknown |
| 400 | `confirm_depth_out_of_range` | Outside 2 to 96 on Ethereum |
| 400 | `unsupported_chain`, `unsupported_network` | Only `eth` / `mainnet` is accepted |
| 400 | `target_url_*` | The [target URL rules](retries-and-endpoint-protection.md#target-url-rules) |
| 400 | `filter_required`, `filter_malformed`, `filter_not_object`, `filter_ambiguous_node`, `filter_empty_group`, `filter_empty_leaf`, `filter_invalid_leaf`, `filter_invalid_dir`, `filter_invalid_hex`, `filter_invalid_position`, `filter_invalid_list_ref`, `filter_too_deep`, `filter_too_large`, `filter_trigger_mismatch`, `filter_reserved_field`, `unknown_filter_leaf` | The filter tree is invalid (depth up to 8, up to 256 nodes) or does not suit the trigger. An unknown key in a filter node is `filter_invalid_leaf` in a leaf and `filter_malformed` next to `all` / `any` / `not` |
| 400 | `filter_would_be_empty` | The update would leave the webhook with no condition |
| 400 | `abi_filter_not_supported` | The `abi` leaf is not supported |
| 400 | `invalid_webhook_name`, `invalid_list_name` | Empty after trimming, over 128 bytes, or control characters |
| 400 | `list_name_taken` | You already have a list with this name |
| 400 | `invalid_batching` | Negative batching values |
| 400 | `invalid_plan_limits` | Your plan's limits are misconfigured — contact support |
| 400 | `invalid_limit`, `invalid_cursor`, `invalid_status`, `invalid_from`, `invalid_to`, `invalid_time_range` | Query parameters of list and delivery-history requests |
| 401 | — | Missing or invalid API key |
| 403 | `plan_required` | A team account without a plan: create a webhook, create a list, resume a `paused_plan` webhook |
| 404 | `not_found` | No such webhook (or not yours); also an unknown path or method |
| 404 | `list_not_found` | No such address list (or not yours) |
| 409 | `cap_active_webhooks_exceeded` | The plan's active-webhook limit |
| 409 | `cap_addresses_exceeded` | The plan's addresses-per-webhook limit |
| 409 | `cap_account_addresses_exceeded` | The account-wide address limit |
| 409 | `cap_inline_addresses_exceeded`, `cap_list_refs_exceeded` | Addresses written into the filter / lists per webhook |
| 409 | `cap_addresses_per_list_exceeded`, `cap_address_lists_exceeded` | Addresses per list / lists per account |
| 409 | `list_in_use` | Deleting a list a webhook still references |
| 409 | `insufficient_balance` | Resuming a `paused_no_balance` webhook while the balance is empty |
| 409 | `webhook_not_active` | A test event for a webhook that is not `active` |
| 413 | `request_too_large` | The body is too large. Above about 5 MiB the gateway answers `413` itself, as plain text without a code |
| 500 | `internal` | An error on our side |
| 502 | — | A GetBlock service behind the API did not answer — retry later |
| 503 | `plan_caps_unavailable` | Your plan could not be checked right now — retry |
| 503 | `test_event_unavailable` | The test event was not confirmed in time; it may still arrive |
| 503 | `delivery_log_not_configured`, `database_not_configured` | The service is temporarily unavailable — retry later |
