---
description: >-
  Create a webhook that sends matching Ethereum events to your endpoint.
  Complete guide on how to use POST /webhooks in GetBlock Webhooks
  documentation.
---

# Create webhook - Webhooks

`POST /webhooks` creates a webhook that sends every on-chain event matching its trigger and filter to your `target_url`, and returns the webhook together with its signing **secret**. The webhook is `active` as soon as it is created.

{% code overflow="wrap" %}
```
POST https://public-api.getblock.io/api/v1/webhooks
```
{% endcode %}

{% hint style="warning" %}
**The secret is shown only once.** Store it now: no other endpoint returns it. You need it to [verify every delivery](../verifying-signatures.md). If you lose it, [rotate it](rotate-webhook-secret.md).
{% endhint %}

## Parameters

The body is a JSON object. Unknown fields are refused with `400 malformed_body`.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `chain` | string | Yes | `eth`. |
| `network` | string | No | `mainnet`, the default. |
| `trigger_type` | string | Yes | `address_activity`, `log_event`, or `tx_confirmation`. See [Triggers](../triggers-and-filters.md#triggers). |
| `phases` | array of strings | Yes | `["unconfirmed"]`, `["confirmed"]`, or both. Ignored by `tx_confirmation`, but still required: send `["confirmed"]`. |
| `target_url` | string | Yes | An `https` URL on a public domain, without credentials or a fragment. See [Target URL rules](../retries-and-endpoint-protection.md#target-url-rules). |
| `filters` | object | One of `filters`, `addresses`, `list_refs` | The filter tree. See [Filters](../triggers-and-filters.md#filters). |
| `addresses` | array of strings | One of `filters`, `addresses`, `list_refs` | Addresses to watch in both directions, as 0x-prefixed 20-byte hex. Stored in the webhook's own list. |
| `list_refs` | array of strings | One of `filters`, `addresses`, `list_refs` | Ids of your [address lists](create-address-list.md) (`al_…`) to watch in both directions. |
| `confirm_depth` | integer or null | No | Blocks before the `confirmed` delivery: 2 to 96. Leave it out or send `null` for the default, 96. |
| `name` | string | No | Display name, at most 128 bytes. Surrounding spaces are trimmed. |
| `batching` | object | No | Accepted and stored, but has no effect in this version. |

You can combine the three condition fields. `addresses` and `list_refs` become one `address` leaf with `"dir": "any"` and `"src": "sugar"`; with `filters` as well, the stored tree is `{"all": [<your tree>, <that leaf>]}`. See [Shortcuts for addresses and lists](../triggers-and-filters.md#shortcuts-for-addresses-and-lists).

## Request Example

{% tabs %}
{% tab title="cURL" %}
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

{% tab title="JavaScript" %}
{% code overflow="wrap" %}
```javascript
const response = await fetch('https://public-api.getblock.io/api/v1/webhooks', {
  method: 'POST',
  headers: {
    Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'usdt-transfers',
    chain: 'eth',
    network: 'mainnet',
    trigger_type: 'log_event',
    phases: ['unconfirmed', 'confirmed'],
    target_url: 'https://hooks.example.io/getblock',
    filters: {
      all: [
        { type: 'contract', in: ['0xdac17f958d2ee523a2206206994597c13d831ec7'] },
        { type: 'topic', position: 0, in: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'] }
      ]
    }
  })
});

const webhook = await response.json();
console.log(webhook.id, webhook.secret); // store the secret now
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import os
import requests

response = requests.post(
    "https://public-api.getblock.io/api/v1/webhooks",
    headers={"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"},
    json={
        "name": "usdt-transfers",
        "chain": "eth",
        "network": "mainnet",
        "trigger_type": "log_event",
        "phases": ["unconfirmed", "confirmed"],
        "target_url": "https://hooks.example.io/getblock",
        "filters": {
            "all": [
                {"type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"]},
                {"type": "topic", "position": 0, "in": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]},
            ]
        },
    },
)

webhook = response.json()
print(webhook["id"], webhook["secret"])  # store the secret now
```
{% endcode %}
{% endtab %}
{% endtabs %}

More request bodies, for wallet activity and transaction confirmations, are in [Triggers and Filters → Examples](../triggers-and-filters.md#examples).

## Response Example

`201 Created`:

{% code overflow="wrap" %}
```json
{
  "id": "wh_4Qk7Xz9pLm2R",
  "name": "usdt-transfers",
  "status": "active",
  "chain": "eth",
  "network": "mainnet",
  "trigger_type": "log_event",
  "filters": {
    "all": [
      { "type": "contract", "in": ["0xdac17f958d2ee523a2206206994597c13d831ec7"] },
      { "type": "topic", "position": 0, "in": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"] }
    ]
  },
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

## Response Parameters

The answer is the [webhook object](objects-and-conventions.md#the-webhook-object) plus `secret`:

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The new webhook's id (`wh_…`). Use it in every other `/webhooks/{id}` call. |
| `status` | string | `active`. |
| `filters` | object | The stored filter tree, including the leaf built from `addresses` and `list_refs`. Addresses are in lower case. |
| `confirm_depth` | integer or null | `null` when you left it out: the chain default, 96, applies. |
| `target_url` | string | Your URL in canonical form. Compare against this value, not the one you sent. |
| `version` | integer | `1`. |
| `secret` | string | The signing secret (`whsec_…`). **Shown only in this answer.** |

## Use Cases

* Notify a payment service of incoming and outgoing transfers of its deposit wallets
* Track every `Transfer` of one ERC-20 token
* Release goods or credit a balance once a transaction is 12 blocks deep
* Feed contract events into a monitoring or alerting tool without running a node

## Error Handling

| HTTP | Code | Cause |
| --- | --- | --- |
| 400 | `empty_body`, `malformed_body` | No body, invalid JSON, an unknown field, or a wrong type. |
| 400 | `invalid_trigger_type`, `trigger_type_not_supported` | Unknown `trigger_type`, or `dropped_replaced`, which is not available. |
| 400 | `invalid_phases` | `phases` is empty or holds an unknown value. |
| 400 | `confirm_depth_out_of_range` | `confirm_depth` is outside 2 to 96. |
| 400 | `unsupported_chain`, `unsupported_network` | Anything but `eth` / `mainnet`. |
| 400 | `target_url_*` | `target_url` breaks one of the [target URL rules](../retries-and-endpoint-protection.md#target-url-rules). |
| 400 | `filter_*`, `unknown_filter_leaf`, `abi_filter_not_supported` | The filter tree is invalid or does not suit the trigger. See [Error codes](error-codes.md). |
| 400 | `invalid_webhook_name`, `invalid_batching` | `name` is empty after trimming, too long, or has control characters; `batching` has negative values. |
| 403 | `plan_required` | A team account without a plan. |
| 404 | `list_not_found` | A `list_refs` entry is not one of your address lists. |
| 409 | `cap_active_webhooks_exceeded`, `cap_addresses_exceeded`, `cap_inline_addresses_exceeded`, `cap_list_refs_exceeded`, `cap_account_addresses_exceeded` | The webhook does not fit your plan. Check [`GET /webhooks/limits`](get-webhook-limits.md). |
| 413 | — | The body is larger than about 5 MiB. Move addresses into [address lists](create-address-list.md). |
