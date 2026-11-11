---
description: >-
  Full reference for the GetBlock Platform API: base URL, authentication, and
  every endpoint for access tokens, nodes, subscriptions, balance, pricing,
  and the protocol catalog.
---

# API Reference

This section documents every endpoint of the GetBlock Platform API. Each endpoint has its own page covering its parameters, a sample request and response, and every field it returns. The interactive schema is in the [OpenAPI reference](https://public-api.getblock.io/docs/v1).

### Quickstart

Create an archive JSON-RPC endpoint for Ethereum Mainnet:

{% tabs %}
{% tab title="Request" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://public-api.getblock.io/api/v1/tokens \
  -H "Authorization: Bearer $GETBLOCK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"protocol":"eth","network":"mainnet","api":"json-rpc","mode":"archive"}'
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
The API answers `201 Created` with the token and its ready-to-use `endpoint`:

{% code overflow="wrap" %}
```json
{
  "id": "9b2796d7b4c448de83b01644bb93f625",
  "name": "eth-9b279",
  "status": "active",
  "namespace": "shared",
  "protocol": "eth",
  "network": "mainnet",
  "api": "json-rpc",
  "mode": "archive",
  "region": "eu-central-1",
  "addon": "",
  "dedicated_node_id": "",
  "limitless_node_id": "",
  "endpoint": "https://shared.eu-central-1.getblock.io/9b2796d7b4c448de83b01644bb93f625/",
  "created_at": "2026-10-02T13:19:13Z",
  "updated_at": "2026-10-02T13:19:13Z"
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
Create a key under [**Settings → API Keys**](https://account.getblock.io/settings/api-keys) in your GetBlock account. Keys start with `gb_`. Treat the key as a secret: anyone who holds it can create and delete your access tokens. The examples on these pages read it from the `GETBLOCK_API_KEY` environment variable.
{% endhint %}

A missing or invalid key answers `401`.

### Account

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/me`](get-me.md) | `GET` | The user id behind the API key |

### Access tokens

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/tokens`](list-tokens.md) | `GET` | List your access tokens, most recently updated first |
| [`/tokens`](create-token.md) | `POST` | Create a Shared Node access token |
| [`/tokens/{id}`](get-token.md) | `GET` | Get one access token |
| [`/tokens/{id}`](delete-token.md) | `DELETE` | Delete an access token |
| [`/tokens/{id}/rotate`](rotate-token.md) | `POST` | Replace a token with a new one of the same configuration |

### Dedicated Nodes

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/dedicated`](list-dedicated-nodes.md) | `GET` | List your Dedicated Nodes |
| [`/dedicated/{id}`](get-dedicated-node.md) | `GET` | Get one Dedicated Node, with the APIs and add-ons it serves |
| [`/dedicated/{id}/tokens`](create-dedicated-node-token.md) | `POST` | Create an access token bound to a Dedicated Node |

### Limitless Nodes

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/limitless`](list-limitless-nodes.md) | `GET` | List your Limitless Nodes |
| [`/limitless/{id}`](get-limitless-node.md) | `GET` | Get one Limitless Node |
| [`/limitless/{id}/tokens`](create-limitless-node-token.md) | `POST` | Create an access token bound to a Limitless Node |

### Plan and subscriptions

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/subscription`](get-subscription.md) | `GET` | Your current plan and its limits |
| [`/subscriptions`](list-subscriptions.md) | `GET` | Your plan, add-on, and package subscriptions |
| [`/subscriptions/{id}`](get-subscription-by-id.md) | `GET` | Get one subscription |

### Balance and pricing

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/balance`](get-balance.md) | `GET` | Your CU balance and prepaid USD credits |
| [`/pricing`](get-pricing.md) | `GET` | What each plan costs your account |

### Catalog

| Endpoint | Method | Description |
| --- | --- | --- |
| [`/protocols`](list-protocols.md) | `GET` | Protocols available on Shared Nodes |
| [`/protocols/{id}`](get-protocol.md) | `GET` | One protocol's networks, sync modes, API types, regions, and add-ons |
| [`/addons`](list-addons.md) | `GET` | Add-ons available on Shared Nodes |

### Choosing an endpoint

<table data-search="false"><thead><tr><th>If you need to</th><th>Use</th></tr></thead><tbody><tr><td>Check that an API key works</td><td><a href="get-me.md"><code>GET /me</code></a></td></tr><tr><td>Find which networks, APIs, and regions a chain supports</td><td><a href="get-protocol.md"><code>GET /protocols/{id}</code></a></td></tr><tr><td>Create a Shared Node endpoint</td><td><a href="create-token.md"><code>POST /tokens</code></a></td></tr><tr><td>Create an endpoint on a Dedicated or Limitless Node</td><td><a href="create-dedicated-node-token.md"><code>POST /dedicated/{id}/tokens</code></a> or <a href="create-limitless-node-token.md"><code>POST /limitless/{id}/tokens</code></a></td></tr><tr><td>Replace a leaked endpoint URL</td><td><a href="rotate-token.md"><code>POST /tokens/{id}/rotate</code></a></td></tr><tr><td>Find every endpoint for one chain</td><td><a href="list-tokens.md"><code>GET /tokens?protocol=eth</code></a></td></tr><tr><td>Check how many tokens your plan allows</td><td><a href="get-subscription.md"><code>GET /subscription</code></a></td></tr><tr><td>Alert before CU runs out</td><td><a href="get-balance.md"><code>GET /balance</code></a></td></tr><tr><td>Compare plans at the price your account would pay</td><td><a href="get-pricing.md"><code>GET /pricing</code></a></td></tr></tbody></table>

### Reference

| Page | What it covers |
| --- | --- |
| [Objects and Conventions](objects-and-conventions.md) | The access token, node, and subscription objects, pagination, filters, ownership, and errors |
