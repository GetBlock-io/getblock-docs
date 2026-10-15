---
description: >-
  The GetBlock Platform API is a REST interface to your GetBlock account:
  issue and rotate access tokens, read plan limits and balances, inspect
  Dedicated and Limitless nodes, and browse the protocol catalog from code.
---

# Overview

**The GetBlock Platform API manages your GetBlock account over REST.** Tasks that otherwise happen in the [GetBlock dashboard](https://account.getblock.io/) — issuing endpoint tokens, rotating a leaked one, checking plan limits, reading the CU balance, listing nodes — become HTTP calls that a script, a CI job, or an AI agent can make.

{% code overflow="wrap" %}
```
https://public-api.getblock.io/api/v1
```
{% endcode %}

Every request carries one header, `Authorization: Bearer <API-KEY>`, with a key created in the dashboard. Responses cover only the account behind that key.

### What the API covers

| Area | What it does | Endpoints |
| --- | --- | --- |
| **Access tokens** | Create, list, inspect, rotate, and delete the tokens inside your RPC endpoint URLs | [`/tokens`](api-reference/README.md#access-tokens) |
| **Dedicated Nodes** | List your Dedicated Nodes and issue tokens bound to one | [`/dedicated`](api-reference/README.md#dedicated-nodes) |
| **Limitless Nodes** | List your Limitless Nodes and issue tokens bound to one | [`/limitless`](api-reference/README.md#limitless-nodes) |
| **Plan and subscriptions** | Read the current plan and its limits, and every active subscription | [`/subscription`, `/subscriptions`](api-reference/README.md#plan-and-subscriptions) |
| **Balance** | Read the CU balance and prepaid USD credits | [`/balance`](api-reference/README.md#balance-and-pricing) |
| **Pricing** | Read what each plan costs your account, discounts applied | [`/pricing`](api-reference/README.md#balance-and-pricing) |
| **Catalog** | List the protocols, networks, sync modes, API types, regions, and add-ons available on Shared Nodes | [`/protocols`, `/addons`](api-reference/README.md#catalog) |

### Common uses

* **Rotate a leaked token from CI.** [`POST /tokens/{id}/rotate`](api-reference/rotate-token.md) issues a replacement with the same configuration and a new endpoint URL. The old token stops working when the call returns, so one job can rotate and redeploy.
* **Provision endpoints in deployment scripts.** [`POST /tokens`](api-reference/create-token.md) returns a ready-to-use endpoint URL for any protocol, network, and API type in the [catalog](api-reference/get-protocol.md).
* **Watch plan limits and balance.** [`GET /subscription`](api-reference/get-subscription.md) and [`GET /balance`](api-reference/get-balance.md) feed a dashboard or an alert before CU runs out.
* **Let an AI agent manage endpoints.** The catalog endpoints describe every valid configuration, so an agent can assemble a correct token request without guessing.

### What stays in the dashboard

This first version leaves some operations to the [GetBlock dashboard](https://account.getblock.io/):

* Creating API keys
* Ordering Dedicated Nodes and Limitless Nodes
* Changing plans and buying CU packages
* Managing teams
* Downloading invoices

Usage statistics and billing endpoints are planned for a later version.

{% hint style="warning" %}
**An API key acts on the whole account.** It can delete tokens, which breaks every client using their endpoint URLs. Keep the key in a secrets manager or an environment variable, never in source code.
{% endhint %}

### OpenAPI specification

The full OpenAPI 3.0 specification is published at [public-api.getblock.io/docs/v1](https://public-api.getblock.io/docs/v1). Try requests in the browser there, or import the spec into Postman or an OpenAPI code generator.

### Next steps

* [Getting Started](getting-started.md): create an API key and make your first call
* [API Reference](api-reference/README.md): every endpoint, with parameters and examples
