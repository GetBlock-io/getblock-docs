---
description: >-
  The GetBlock Platform API is a REST interface to your GetBlock account: issue
  and rotate access tokens, read plan limits and balances, inspect Dedicated and
  Limitless nodes, and browse the protocol ca
---

# Overview

**The GetBlock Platform API allows you to manage your GetBlock account over REST.** Tasks that otherwise happen in the [GetBlock dashboard](https://account.getblock.io/) — issuing endpoint tokens, rotating a leaked one, checking plan limits, reading the CU balance, listing nodes — become HTTP calls that a script, a CI job, or an AI agent can make.

{% code overflow="wrap" %}
```
https://public-api.getblock.io/api/v1
```
{% endcode %}

Every request carries one header, `Authorization: Bearer <API-KEY>`, with a key created in the dashboard. Responses cover only the account behind that key.

### What the API covers

<table data-search="false"><thead><tr><th>Area</th><th>What it does</th><th>Endpoints</th></tr></thead><tbody><tr><td><strong>Access tokens</strong></td><td>Create, list, inspect, rotate, and delete the tokens inside your RPC endpoint URLs</td><td><a href="api-reference/#access-tokens"><code>/tokens</code></a></td></tr><tr><td><strong>Dedicated Nodes</strong></td><td>List your Dedicated Nodes and issue tokens bound to one</td><td><a href="api-reference/#dedicated-nodes"><code>/dedicated</code></a></td></tr><tr><td><strong>Limitless Nodes</strong></td><td>List your Limitless Nodes and issue tokens bound to one</td><td><a href="api-reference/#limitless-nodes"><code>/limitless</code></a></td></tr><tr><td><strong>Plan and subscriptions</strong></td><td>Read the current plan and its limits, and every active subscription</td><td><a href="api-reference/#plan-and-subscriptions"><code>/subscription</code>, <code>/subscriptions</code></a></td></tr><tr><td><strong>Balance</strong></td><td>Read the CU balance and prepaid USD credits</td><td><a href="api-reference/#balance-and-pricing"><code>/balance</code></a></td></tr><tr><td><strong>Pricing</strong></td><td>Read what each plan costs your account, discounts applied</td><td><a href="api-reference/#balance-and-pricing"><code>/pricing</code></a></td></tr><tr><td><strong>Catalog</strong></td><td>List the protocols, networks, sync modes, API types, regions, and add-ons available on Shared Nodes</td><td><a href="api-reference/#catalog"><code>/protocols</code>, <code>/addons</code></a></td></tr></tbody></table>

### Common uses

* **Rotate a leaked token from CI.** [`POST /tokens/{id}/rotate`](api-reference/rotate-token.md) issues a replacement with the same configuration and a new endpoint URL. The old token stops working when the call returns, so a job can rotate and redeploy it.
* **Provision endpoints in deployment scripts.** [`POST /tokens`](api-reference/create-token.md) returns a ready-to-use endpoint URL for any protocol, network, and API type in the [catalog](api-reference/get-protocol.md).
* **Watch plan limits and balance.** [`GET /subscription`](api-reference/get-subscription.md) and [`GET /balance`](api-reference/get-balance.md) feed a dashboard or an alert before CU runs out.
* **Let an AI agent manage endpoints:** The catalog endpoints describe all valid configurations, so an agent can assemble a correct token request without guessing.

### What stays in the dashboard

This first version leaves some operations to the [GetBlock dashboard](https://account.getblock.io/):

* Creating API keys
* Ordering Dedicated Nodes and Limitless Nodes
* Changing plans and buying CU packages
* Managing teams
* Downloading invoices

Usage statistics and billing endpoints are planned for a later version.

{% hint style="warning" %}
**An API key applies to the entire account.** It can delete tokens, which breaks every client using their endpoint URLs. Keep the key in a Secrets Manager or an environment variable, never in source code.
{% endhint %}

### OpenAPI specification

The full OpenAPI 3.0 specification is published at [public-api.getblock.io/docs/v1](https://public-api.getblock.io/docs/v1). Try requests in the browser there, or import the spec into Postman or an OpenAPI code generator.

### Next steps

* [Getting Started](getting-started.md): create an API key and make your first call
* [API Reference](api-reference/): every endpoint, with parameters and examples
