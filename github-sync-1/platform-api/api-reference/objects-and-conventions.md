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

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td>The access token itself, the credential inside <code>endpoint</code>. Treat it as a secret.</td></tr><tr><td><code>name</code></td><td>string</td><td>Generated when the token is created. It cannot be chosen.</td></tr><tr><td><code>status</code></td><td>string</td><td><code>active</code> for a usable token; <code>locked</code> for one suspended by plan or billing state.</td></tr><tr><td><code>namespace</code></td><td>string</td><td>Access type the token was issued for, such as <code>shared</code>.</td></tr><tr><td><code>protocol</code></td><td>string</td><td>Protocol id, such as <code>eth</code>.</td></tr><tr><td><code>network</code></td><td>string</td><td>Network id, such as <code>mainnet</code>.</td></tr><tr><td><code>api</code></td><td>string</td><td>API type served on the endpoint, such as <code>json-rpc</code>.</td></tr><tr><td><code>mode</code></td><td>string</td><td>Sync mode, <code>full</code> or <code>archive</code>. Empty when it does not apply.</td></tr><tr><td><code>region</code></td><td>string</td><td>Region serving the endpoint, such as <code>eu-central-1</code> for Frankfurt, <code>us-east-1</code> for New York, <code>ap-southeast-1</code> for Singapore</td></tr><tr><td><code>addon</code></td><td>string</td><td>Add-on bound to the token. Empty when none.</td></tr><tr><td><code>dedicated_node_id</code></td><td>string</td><td>Set only for tokens bound to a Dedicated Node. Empty otherwise.</td></tr><tr><td><code>limitless_node_id</code></td><td>string</td><td>Set only for tokens bound to a Limitless Node. Empty otherwise.</td></tr><tr><td><code>endpoint</code></td><td>string</td><td>Ready-to-use RPC URL for the token.</td></tr><tr><td><code>created_at</code>, <code>updated_at</code></td><td>string</td><td>RFC 3339 UTC times.</td></tr></tbody></table>

### The Dedicated Node object

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td>The node id, used in the <code>/dedicated/{id}</code> routes.</td></tr><tr><td><code>name</code></td><td>string</td><td>The node's name.</td></tr><tr><td><code>status</code></td><td>string</td><td><code>created</code>, <code>deploying</code>, <code>active</code>, or <code>disabled</code>.</td></tr><tr><td><code>protocol</code></td><td>string</td><td>Protocol id.</td></tr><tr><td><code>network</code></td><td>string</td><td>Network id.</td></tr><tr><td><code>sync_mode</code></td><td>string</td><td><code>full</code> or <code>archive</code>.</td></tr><tr><td><code>client</code></td><td>string</td><td>The node's client software, such as <code>geth</code>.</td></tr><tr><td><code>region</code></td><td>string</td><td>Region the node runs in.</td></tr><tr><td><code>performance_grade</code></td><td>string</td><td><code>standard</code> or <code>high</code>. See <a href="../../getting-started/endpoint-setup/configuring-dedicated-nodes/dedicated-node-performance-tiers.md">Dedicated node performance tiers</a>.</td></tr><tr><td><code>apis</code></td><td>array of strings</td><td>API types the node serves: the valid values of <code>api</code> when creating a token for it.</td></tr><tr><td><code>addons</code></td><td>array of strings</td><td>Add-ons the node serves: the valid values of <code>addon</code> when creating a token for it.</td></tr><tr><td><code>created_at</code>, <code>updated_at</code></td><td>string</td><td>RFC 3339 UTC times.</td></tr></tbody></table>

### The Limitless Node object

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td>The node id, used in the <code>/limitless/{id}</code> routes.</td></tr><tr><td><code>status</code></td><td>string</td><td>The node subscription's status, such as <code>active</code> or <code>pending</code>.</td></tr><tr><td><code>protocol</code></td><td>string</td><td>Protocol id.</td></tr><tr><td><code>network</code></td><td>string</td><td>Network id.</td></tr><tr><td><code>region</code></td><td>string</td><td>Region serving the node.</td></tr><tr><td><code>mode</code></td><td>string</td><td>Sync mode, <code>full</code> or <code>archive</code>.</td></tr><tr><td><code>rps_limit</code></td><td>integer</td><td>The node's guaranteed requests per second.</td></tr><tr><td><code>grade</code></td><td>string</td><td>The node's tier, such as <code>full-growth</code>.</td></tr><tr><td><code>apis</code></td><td>array of strings</td><td>API types your existing tokens on the node serve. Not the full set: any API in the <a href="get-protocol.md">catalog</a> for the node's protocol may be requested.</td></tr></tbody></table>

### The subscription object

A subscription is a plan, an enterprise plan, or a request package. Each is its own record, told apart by `product_type`. Dedicated and Limitless Nodes are not subscriptions in this API: they are under `/dedicated` and `/limitless`.

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>string</td><td>The subscription record, unique per subscription.</td></tr><tr><td><code>product_type</code></td><td>string</td><td><code>plan</code>, <code>enterprise_plan</code>, or <code>request_package</code>.</td></tr><tr><td><code>product_id</code></td><td>string</td><td>The product, such as <code>pro</code> for a plan. Shared by everyone on the same product.</td></tr><tr><td><code>status</code></td><td>string</td><td><code>active</code> and <code>grace_period</code> are serving; <code>cancelled</code> and <code>awaiting_render</code> are not.</td></tr><tr><td><code>period</code></td><td>string</td><td>Billing period, such as <code>1m</code> or <code>12m</code>.</td></tr><tr><td><code>type</code></td><td>string</td><td><code>regular</code> or <code>trial</code>.</td></tr><tr><td><code>started_at</code>, <code>paid_until</code>, <code>current_period_end</code></td><td>string</td><td>RFC 3339 UTC times.</td></tr></tbody></table>

### Pagination

List endpoints page with `limit` and `offset` and return a `total`:

{% code overflow="wrap" %}
```json
{ "total": 18, "limit": 20, "offset": 0, "tokens": [ … ] }
```
{% endcode %}

| Parameter | Type    | Description                                                           |
| --------- | ------- | --------------------------------------------------------------------- |
| `limit`   | integer | Page size. Default 20, at most 100; a larger value is treated as 100. |
| `offset`  | integer | Number of matching items to skip. Default 0.                          |

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

| HTTP | When                                                                                                                |
| ---- | ------------------------------------------------------------------------------------------------------------------- |
| 400  | A required field is missing, or the requested combination of protocol, network, API, mode, and region is not served |
| 401  | Missing or invalid API key                                                                                          |
| 403  | Your plan does not allow it: the token limit is reached, or the network or region requires a paid plan              |
| 404  | No such token, node, subscription, or protocol among yours; also another account's resource                         |
| 429  | Too many requests. Wait and retry                                                                                   |
| 502  | A GetBlock service behind the API did not answer or returned an error. Retry later                                  |
