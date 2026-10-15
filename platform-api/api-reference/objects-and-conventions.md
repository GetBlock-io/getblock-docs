---
description: >-
  The access token, node, and subscription objects, pagination, filters,
  ownership rules, and error format shared by every endpoint of the GetBlock
  Platform API.
---

# Objects and Conventions

These objects and rules apply to every endpoint of the [Platform API](./). Each endpoint page links here for the details it shares with the others.

### The access token object

Every endpoint that returns an access token returns this object: list, details, create, and rotate.

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The access token itself, the credential inside `endpoint`. Treat it as a secret. |
| `name` | string | Generated when the token is created. It cannot be chosen. |
| `status` | string | `active` for a usable token; `locked` for one suspended by plan or billing state. |
| `namespace` | string | Access type the token was issued for, such as `shared`. |
| `protocol` | string | Protocol id, such as `eth`. |
| `network` | string | Network id, such as `mainnet`. |
| `api` | string | API type served on the endpoint, such as `json-rpc`. |
| `mode` | string | Sync mode, `full` or `archive`. Empty when it does not apply. |
| `region` | string | Region serving the endpoint, such as `eu-central-1`. |
| `addon` | string | Add-on bound to the token. Empty when none. |
| `dedicated_node_id` | string | Set only for tokens bound to a Dedicated Node. Empty otherwise. |
| `limitless_node_id` | string | Set only for tokens bound to a Limitless Node. Empty otherwise. |
| `endpoint` | string | Ready-to-use RPC URL for the token. |
| `created_at`, `updated_at` | string | RFC 3339 UTC times. |

### The Dedicated Node object

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The node id, used in the `/dedicated/{id}` routes. |
| `name` | string | The node's name. |
| `status` | string | `created`, `deploying`, `active`, or `disabled`. |
| `protocol` | string | Protocol id. |
| `network` | string | Network id. |
| `sync_mode` | string | `full` or `archive`. |
| `client` | string | The node's client software, such as `geth`. |
| `region` | string | Region the node runs in. |
| `performance_grade` | string | `standard` or `high`. See [Dedicated node performance tiers](../../getting-started/endpoint-setup/configuring-dedicated-nodes/dedicated-node-performance-tiers.md). |
| `apis` | array of strings | API types the node serves: the valid values of `api` when creating a token for it. |
| `addons` | array of strings | Add-ons the node serves: the valid values of `addon` when creating a token for it. |
| `created_at`, `updated_at` | string | RFC 3339 UTC times. |

### The Limitless Node object

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The node id, used in the `/limitless/{id}` routes. |
| `status` | string | The node subscription's status, such as `active` or `pending`. |
| `protocol` | string | Protocol id. |
| `network` | string | Network id. |
| `region` | string | Region serving the node. |
| `mode` | string | Sync mode, `full` or `archive`. |
| `rps_limit` | integer | The node's guaranteed requests per second. |
| `grade` | string | The node's tier, such as `full-growth`. |
| `apis` | array of strings | API types your existing tokens on the node serve. Not the full set: any API in the [catalog](get-protocol.md) for the node's protocol may be requested. |

### The subscription object

A subscription is a plan, an enterprise plan, or a request package. Each is its own record, told apart by `product_type`. Dedicated and Limitless Nodes are not subscriptions in this API: they are under `/dedicated` and `/limitless`.

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | The subscription record, unique per subscription. |
| `product_type` | string | `plan`, `enterprise_plan`, or `request_package`. |
| `product_id` | string | The product, such as `pro` for a plan. Shared by everyone on the same product. |
| `status` | string | `active` and `grace_period` are serving; `cancelled` and `awaiting_render` are not. |
| `period` | string | Billing period, such as `1m` or `12m`. |
| `type` | string | `regular` or `trial`. |
| `started_at`, `paid_until`, `current_period_end` | string | RFC 3339 UTC times. |

### Pagination

List endpoints page with `limit` and `offset` and return a `total`:

{% code overflow="wrap" %}
```json
{ "total": 18, "limit": 20, "offset": 0, "tokens": [ … ] }
```
{% endcode %}

| Parameter | Type | Description |
| --- | --- | --- |
| `limit` | integer | Page size. Default 20, at most 100; a larger value is treated as 100. |
| `offset` | integer | Number of matching items to skip. Default 0. |

`total` counts every item that matches the filters, before `limit` and `offset`. More pages exist while `offset + limit < total`.

{% code title="paginate.js" overflow="wrap" %}
```javascript
const tokens = [];
let offset = 0;
let total;

do {
  const response = await fetch(`https://public-api.getblock.io/api/v1/tokens?limit=100&offset=${offset}`, {
    headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
  });
  const page = await response.json();
  tokens.push(...page.tokens);
  total = page.total;
  offset += page.limit;
} while (offset < total);
```
{% endcode %}

### Filters

List endpoints accept filters as query parameters, such as `protocol`, `network`, `region`, and `status`. Filters combine with AND and are case-insensitive. Each endpoint page lists the filters it accepts.

### Ownership

Responses cover only the account behind the API key. A token, node, or subscription that belongs to another account answers `404`, exactly like an unknown id. The API never reveals whether another account's id exists.

### Creation rules

Node endpoints have no create or delete: Dedicated and Limitless Nodes are ordered in the [dashboard](https://account.getblock.io/). The API lists and inspects them, and issues tokens bound to them.

Your plan limits what you can create. It caps the number of tokens and decides which networks and regions are open to you. The `max_tokens` limit is in [`GET /subscription`](get-subscription.md); paid-only regions are flagged per network in [`GET /protocols/{id}`](get-protocol.md).

### Errors

Error responses are JSON with an `error` message that explains the refusal:

{% code overflow="wrap" %}
```json
{ "error": "missing user identity", "request_id": "de53c89b-4ee1-413e-805f-a285ad3e7376" }
```
{% endcode %}

Include `request_id` when you contact support.

| HTTP | When |
| --- | --- |
| 400 | A required field is missing, or the requested combination of protocol, network, API, mode, and region is not served |
| 401 | Missing or invalid API key |
| 403 | Your plan does not allow it: the token limit is reached, or the network or region requires a paid plan |
| 404 | No such token, node, subscription, or protocol among yours; also another account's resource |
| 429 | Too many requests. Wait and retry |
| 502 | A GetBlock service behind the API did not answer or returned an error. Retry later |
