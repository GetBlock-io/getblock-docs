---
description: >-
  GetBlock Webhooks send signed HTTPS POST notifications to your endpoint when
  on-chain events you care about happen on Ethereum: address activity, contract
  logs and transaction confirmations.
---

# Overview

**GetBlock Webhooks push on-chain events to your own HTTPS endpoint, for payment services, wallets, exchanges and monitoring tools.** You describe what to watch — wallet addresses, a contract's events or a transaction's confirmations — and GetBlock sends every matching event to your URL as a signed `POST` request.

There is nothing to poll and no connection to keep open: your server only has to accept HTTPS requests. You manage webhooks in the [GetBlock dashboard](https://account.getblock.io/products/webhooks) or through the [Public API](api-reference/README.md).

### How it works

1. **You create a webhook:** the network, a trigger, a filter that says what to watch, and the `target_url` of your endpoint.
2. **GetBlock matches every new block** against the webhook's filter.
3. **Each matching event is sent as one signed `POST`** — one event per request, with an `X-GetBlock-Signature` header you verify with the webhook's secret.
4. **Your endpoint answers `2xx` within 15 seconds.** If it does not, GetBlock retries: up to 8 attempts over about 52 minutes.

Events come in two phases, and you choose one or both:

* **unconfirmed** — as soon as the event is in a new block. Fastest, but a chain reorganization can still remove it.
* **confirmed** — once the block is `confirm_depth` blocks deep.

When a reorganization removes a block that held an event you received, GetBlock sends a correction with `"removed": true`. See [Handling Chain Reorganizations](handling-chain-reorganizations.md).

### Use cases and triggers

| Use case | Trigger | What you receive |
| --- | --- | --- |
| Incoming and outgoing payments of your wallets | `address_activity` | Native ETH and token transfers to or from the addresses you watch; ERC-20 `Transfer` and `Approval` events come decoded |
| Events of a smart contract, such as the ERC-20 `Transfer` events of one token | `log_event` | Contract logs matched by contract address and/or topics; ERC-20 `Transfer` and `Approval` events come decoded |
| Knowing when a transaction is final enough to act on | `tx_confirmation` | One notification when a transaction of your addresses reaches the confirmation depth |

See [Triggers and Filters](triggers-and-filters.md) for the details and request examples.

### Supported networks

| Network | `chain` | `network` | Confirmation depth |
| --- | --- | --- | --- |
| Ethereum Mainnet | `eth` | `mainnet` | 2 to 96 blocks, default 96 |

Testnets are not available for webhooks.

{% hint style="info" %}
**Coming next:** BNB Smart Chain, Polygon and Base. They cannot be selected yet: a webhook on another chain is refused with `unsupported_chain`.
{% endhint %}

### Pricing

Webhooks are paid from the same CU balance as your RPC requests: **25 CU per delivery attempt**.

* A webhook that takes both phases receives — and pays for — two deliveries per event: 2 × 25 = 50 CU.
* Every retry is an attempt and is billed. An endpoint that fails every attempt costs up to 8 × 25 = 200 CU per event.
* Test deliveries are free.

Your plan sets how many webhooks and addresses you can use and how many events per second are delivered. See [Pricing and Limits](pricing-and-limits.md).

### What this version does not include

{% hint style="warning" %}
* **No replay.** An event whose 8 attempts all fail is not sent again, and events that happen while a webhook is paused are never delivered.
* **No batching.** Every request carries exactly one event.
* **No published list of delivery IP addresses.** Verify the signature instead.
* **Ethereum Mainnet only.** BNB Smart Chain, Polygon and Base come next.
* **No automatic resume.** A webhook paused after repeated failures or an empty CU balance stays paused until you resume it.
{% endhint %}

### Next steps

* [Getting Started](getting-started.md): create a webhook and receive your first signed delivery.
* [Triggers and Filters](triggers-and-filters.md): what fires a delivery and how to describe what to watch.
* [Delivery Format](delivery-format.md): the request and the payload your endpoint receives.
* [Verifying Signatures](verifying-signatures.md): check every request, with Go, JavaScript and Python recipes.
* [Handling Chain Reorganizations](handling-chain-reorganizations.md): de-duplicate events and undo removed ones.
* [Retries and Endpoint Protection](retries-and-endpoint-protection.md): the retry schedule, auto-pause and URL rules.
* [Pricing and Limits](pricing-and-limits.md): CU per attempt, webhook statuses and plan limits.
* [API Reference](api-reference/README.md): every endpoint of the Public API for webhooks and address lists.
* [Webhooks in the dashboard](https://account.getblock.io/products/webhooks)
