---
description: >-
  Full reference for the GetBlock Webhooks API: base URL, authentication,
  every webhook and address-list endpoint, pagination, limits, and error
  codes.
---

# API Reference

This section documents every endpoint of the GetBlock Public API for webhooks and address lists. Everything you do on the [Webhooks page](https://account.getblock.io/products/webhooks) of the dashboard, you can also do through these endpoints.

The API has two groups of endpoints: [**webhooks**](./#webhooks), which say what to watch and where to deliver it, and [**address lists**](./#address-lists), which hold large or shared sets of addresses that webhooks reference. Each endpoint has its own page covering its parameters, a sample request and response, and every field it returns. The interactive schema is in the [Public API reference](https://public-api.getblock.io/docs/v1).

### Quickstart

Create a webhook that delivers every USDT `Transfer` on Ethereum Mainnet to your endpoint:

{% tabs %}
{% tab title="Request" %}
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
{% endtab %}

{% tab title="Response" %}
The API answers `201 Created` with the webhook and its signing `secret`. GetBlock then sends each matching event to `target_url` as a signed `POST`:

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
  "phases": ["unconfirmed", "confirmed"],
  "target_url": "https://hooks.example.io/getblock",
  "endpoint_hash": "3f2a9c0e5b7d41a8c6e2f0b9d8a7c6e5f4d3c2b1a0918273645546372819aabb",
  "batching": { "max_events": 0, "max_wait_ms": 0, "gzip": false },
  "version": 1,
  "created_at": "2026-09-25T10:00:00Z",
  "updated_at": "2026-09-25T10:00:00Z",
  "secret": "whsec_5Hk9pQ2rT7vX1mN4bC8dF3gJ6"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Base URL

{% code overflow="wrap" %}
```
https://public-api.getblock.io/api/v1
```
{% endcode %}

Every path on these pages is relative to this base URL.

### Authentication

Every request needs a GetBlock API key in the `Authorization` header:

```
Authorization: Bearer <API-KEY>
```

{% hint style="info" %}
**Getting your API key**\
\
Create a key under [**Settings → API Keys**](https://account.getblock.io/settings/api-keys) in your GetBlock account. Keys start with `gb_`. Treat the key as a secret: anyone who holds it can manage your webhooks and spend your CU. The examples on these pages read it from the `GETBLOCK_API_KEY` environment variable.
{% endhint %}

A missing or invalid key answers `401`.

### Webhooks

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/webhooks`](create-webhook.md) | `POST` | Create a webhook and get its signing secret |
| [`/webhooks`](list-webhooks.md) | `GET` | List your webhooks, newest first |
| [`/webhooks/limits`](get-webhook-limits.md) | `GET` | Your plan's webhook limits and current usage |
| [`/webhooks/{id}`](get-webhook.md) | `GET` | Get one webhook |
| [`/webhooks/{id}`](update-webhook.md) | `PATCH` | Change some of a webhook's settings |
| [`/webhooks/{id}`](delete-webhook.md) | `DELETE` | Delete a webhook |
| [`/webhooks/{id}/pause`](pause-webhook.md) | `POST` | Stop deliveries |
| [`/webhooks/{id}/resume`](resume-webhook.md) | `POST` | Start deliveries again |
| [`/webhooks/{id}/secret/rotate`](rotate-webhook-secret.md) | `POST` | Issue a new signing secret |
| [`/webhooks/{id}/test`](send-test-event.md) | `POST` | Send a free test event to the endpoint |
| [`/webhooks/{id}/deliveries`](get-webhook-deliveries.md) | `GET` | Failed delivery attempts, newest first |
| [`/webhooks/{id}/stats`](get-webhook-stats.md) | `GET` | Delivery totals and recent rates |
| [`/webhooks/{id}/addresses`](get-webhook-addresses.md) | `GET` | The addresses the webhook was given in `addresses` |

### Address lists

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/address-lists`](create-address-list.md) | `POST` | Create a named list, optionally with its first addresses |
| [`/address-lists`](list-address-lists.md) | `GET` | List your address lists, newest first |
| [`/address-lists/{id}`](get-address-list.md) | `GET` | Get one list, without its addresses |
| [`/address-lists/{id}`](rename-address-list.md) | `PATCH` | Rename a list |
| [`/address-lists/{id}`](delete-address-list.md) | `DELETE` | Delete a list |
| [`/address-lists/{id}/entries`](get-address-list-entries.md) | `GET` | The addresses in a list |
| [`/address-lists/{id}/entries`](add-address-list-entries.md) | `POST` | Add addresses |
| [`/address-lists/{id}/entries`](replace-address-list-entries.md) | `PUT` | Replace all addresses |
| [`/address-lists/{id}/entries/remove`](remove-address-list-entries.md) | `POST` | Remove addresses |

### Choosing an endpoint

<table data-search="false"><thead><tr><th>If you need to</th><th>Use</th></tr></thead><tbody><tr><td>Start receiving events for wallets, a contract, or confirmations</td><td><a href="create-webhook.md"><code>POST /webhooks</code></a></td></tr><tr><td>Check whether a new webhook fits your plan before creating it</td><td><a href="get-webhook-limits.md"><code>GET /webhooks/limits</code></a></td></tr><tr><td>Change the endpoint URL, phases, or what a webhook watches</td><td><a href="update-webhook.md"><code>PATCH /webhooks/{id}</code></a></td></tr><tr><td>Stop deliveries for a while without losing the configuration</td><td><a href="pause-webhook.md"><code>POST /webhooks/{id}/pause</code></a></td></tr><tr><td>Bring back a webhook paused after failures or an empty balance</td><td><a href="resume-webhook.md"><code>POST /webhooks/{id}/resume</code></a></td></tr><tr><td>Replace a lost or leaked signing secret</td><td><a href="rotate-webhook-secret.md"><code>POST /webhooks/{id}/secret/rotate</code></a></td></tr><tr><td>Check that your endpoint receives and verifies deliveries</td><td><a href="send-test-event.md"><code>POST /webhooks/{id}/test</code></a></td></tr><tr><td>Find out why deliveries fail</td><td><a href="get-webhook-deliveries.md"><code>GET /webhooks/{id}/deliveries</code></a></td></tr><tr><td>Count delivered, failed, and rate-limited events</td><td><a href="get-webhook-stats.md"><code>GET /webhooks/{id}/stats</code></a></td></tr><tr><td>Watch more than 1,000 addresses, or share addresses between webhooks</td><td><a href="create-address-list.md"><code>POST /address-lists</code></a>, then <code>list_refs</code> on the webhook</td></tr><tr><td>Sync a list with your own database</td><td><a href="replace-address-list-entries.md"><code>PUT /address-lists/{id}/entries</code></a></td></tr></tbody></table>

### Concepts that apply to every endpoint

#### The webhook object

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

#### The address list object

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | `al_` followed by 12 characters. |
| `name` | string | Unique per account, at most 128 bytes. |
| `entry_count` | integer | How many addresses the list holds. |
| `version` | integer | The list's version number. Compare it to detect changes. |
| `created_at`, `updated_at` | string | RFC 3339 times. |

A list's addresses are not part of the object: read them with [Get address list entries](get-address-list-entries.md).

#### Pagination

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

#### Strict request bodies

Request bodies are JSON and are checked strictly: an unknown field is refused with `400 malformed_body` rather than ignored, so a misspelt option is never dropped silently. Addresses may be sent in any case, EIP-55 checksummed included, and are always returned in lower case.

#### Ownership

A webhook or a list that belongs to someone else answers `404`, exactly like an unknown or malformed id. The API never reveals whether another account's id exists.

#### Error format

Every error answer from these endpoints has the same shape:

{% code overflow="wrap" %}
```json
{ "error": "cap_addresses_exceeded", "request_id": "…" }
```
{% endcode %}

`error` is a stable code to branch on; see [Error codes](./#error-codes). Include `request_id` when you contact support.

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

### Pricing

Webhooks are billed per delivery attempt: **25 CU per attempt**, from the same CU balance as your RPC requests. Test deliveries are free. See [Pricing and Limits](../pricing-and-limits.md).

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
| 400 | `target_url_*` | The [target URL rules](../retries-and-endpoint-protection.md#target-url-rules) |
| 400 | `filter_required`, `filter_malformed`, `filter_not_object`, `filter_ambiguous_node`, `filter_empty_group`, `filter_empty_leaf`, `filter_invalid_leaf`, `filter_invalid_dir`, `filter_invalid_hex`, `filter_invalid_position`, `filter_invalid_list_ref`, `filter_too_deep`, `filter_too_large`, `filter_trigger_mismatch`, `filter_reserved_field`, `unknown_filter_leaf` | The filter tree is invalid (depth up to 8, up to 256 nodes) or does not suit the trigger. An unknown key in a filter node is `filter_invalid_leaf` in a leaf and `filter_malformed` next to `all` / `any` / `not` |
| 400 | `filter_would_be_empty` | The update would leave the webhook with no condition |
| 400 | `abi_filter_not_supported` | The `abi` leaf is not supported |
| 400 | `invalid_webhook_name`, `invalid_list_name` | Empty after trimming, over 128 bytes, or control characters |
| 400 | `list_name_taken` | You already have a list with this name |
| 400 | `invalid_batching` | Negative batching values |
| 400 | `invalid_plan_limits` | Your plan's limits are misconfigured. Contact support |
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
| 502 | — | A GetBlock service behind the API did not answer. Retry later |
| 503 | `plan_caps_unavailable` | Your plan could not be checked right now. Retry |
| 503 | `test_event_unavailable` | The test event was not confirmed in time; it may still arrive |
| 503 | `delivery_log_not_configured`, `database_not_configured` | The service is temporarily unavailable. Retry later |

Each endpoint page lists the codes that endpoint can return.
