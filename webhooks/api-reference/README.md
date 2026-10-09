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

### Reference

| Page | What it covers |
| --- | --- |
| [Objects and Conventions](objects-and-conventions.md) | The webhook and address list objects, pagination, strict request bodies, ownership, and limits |
| [Error Codes](error-codes.md) | The error format and every error code, with its HTTP status |

### Pricing

Webhooks are billed per delivery attempt: **25 CU per attempt**, from the same CU balance as your RPC requests. Test deliveries are free. See [Pricing and Limits](../pricing-and-limits.md).
