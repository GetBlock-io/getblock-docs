---
description: >-
  A dedicated RPC endpoint with guaranteed RPS and no request limits, at a flat
  monthly price. For Ethereum, Solana, Bitcoin and other tier-1 blockchains.
---

# Limitless Node

**Limitless Node** is a dedicated RPC endpoint with a **guaranteed RPS** and **no Compute Unit (CU) limits**. You pick an RPS tier on a single chain, pay a **fixed monthly fee**, and send as many requests as your throughput allows. There's no usage-based billing and no overage charges.

Architecturally, it's a middle layer between Shared RPS and Dedicated Nodes: more throughput and isolation than a shared plan, at a fraction of the cost of a full private node.

{% hint style="success" %}
Limitless Node is the right fit for applications that generate a high volume of RPC requests and need predictable monthly costs without provisioning a full Dedicated Node. Common use cases include indexers, bots, wallet backends, AI agents, and analytics platforms.
{% endhint %}

***

### What's included

Every Limitless Node includes:

* **Guaranteed requests-per-second (RPS)** throughput with no per-method throttling
* **Unlimited requests** within your RPS tier, no CU-based billing
* **No concurrent connection limits** for parallel workloads
* **No restrictions on RPC method usage**. All standard and heavy RPC methods are supported
* **Full node** and **archive node** configurations available for supported chains
* **Isolated capacity** and **dedicated routing** for a single network
* **Multi-region geo-routing** across Europe, the USA, and Asia to minimize latency through geographic proximity
* One instance can have **multiple** [**access tokens**](../authentication-with-access-tokens.md)**,** each a separate credential that can be regenerated or removed independently of the others
* **24/7 customer support** with a response time under 5 minutes
* **Pricing is fixed and predictable** because it is throughput-based rather than usage-based

{% hint style="success" %}
Limitless Node is a standalone product. You don't need an existing Shared or Dedicated plan to use it. You get a fully isolated RPC endpoint from day one.&#x20;
{% endhint %}

***

### RPS tiers and pricing

Pick the tier that matches your current traffic. Within your RPS cap, requests are unlimited. There is no monthly request quota and no CU metering.

| RPS (requests-per-second) Tier | Price (monthly) | Requests  | Best For                |
| ------------------------------ | --------------- | --------- | ----------------------- |
| 25 RPS                         | $150/mo         | Unlimited | Testing & prototypes    |
| 50 RPS                         | $300/mo         | Unlimited | Production dApps        |
| 150 RPS                        | $500/mo         | Unlimited | Indexers & trading bots |
| 300 RPS                        | $1,000/mo       | Unlimited | Enterprise workloads    |

Prices shown are for monthly billing. Annual billing saves 20%. You can move between tiers at any time with no lock-in.

{% hint style="warning" %}
Requests above your RPS limit are rate-limited and return a standard rate-limit response. **We do not charge overage fees**. If you consistently reach your limit, consider upgrading to a higher tier.
{% endhint %}

***

#### Supported chains

One Limitless Node subscription corresponds to one blockchain network. The service is currently available on 10 high-demand blockchains:

| <img src="../../.gitbook/assets/Crypto Symbol=Ethereum.svg" alt="" data-size="line"> Ethereum  | <img src="../../.gitbook/assets/Logo=Solana (1).svg" alt="" data-size="line"> Solana  |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| <img src="../../.gitbook/assets/btc-logo.svg" alt="" data-size="line"> Bitcoin                 | <img src="../../.gitbook/assets/Logo=Tron (1).svg" alt="" data-size="line"> TRON      |
| <img src="../../.gitbook/assets/base-logo.svg" alt="" data-size="line"> Base                   | <img src="../../.gitbook/assets/pol-logo.svg" alt="" data-size="line"> Polygon        |
| <img src="../../.gitbook/assets/arb-logomark (1).svg" alt="" data-size="line"> Arbitrum        | <img src="../../.gitbook/assets/Logo=Avax (1).svg" alt="" data-size="line"> Avalanche |
| <img src="../../.gitbook/assets/Logo=OP (1).svg" alt="" data-size="line"> Optimism             | <img src="../../.gitbook/assets/sui-logomark.svg" alt="" data-size="line"> Sui        |

{% hint style="info" %}
Additional networks may be added over time. If you need a chain that isn't listed yet, a custom RPS ceiling, or multi-region redundancy,[ contact our team](https://getblock.io/contact/).
{% endhint %}

***

### Deploy a Limitless Node

For now, our team provisions every Limitless Node individually so your instance is configured to your workload from day one. Self-serve setup from the dashboard is coming soon.&#x20;

To get started:

1. Contact our team through a contact from on our website or your account.&#x20;
2. Specify the requirements: blockchain network, desired RPS tier, node mode (full or archive), and preferred hosting region.
3. We finalize your configuration and provision a dedicated Limitless Node.
4. Receive your RPC endpoint, generate [access token(s)](https://docs.getblock.io/getting-started/authentication-with-access-tokens). Connect your application and start sending requests.

Our team can also recommend the right tier based on your workload. <a href="https://getblock.io/contact/" class="button primary">Get a free consultation.</a>&#x20;

#### Next steps

* [Endpoint setup](../endpoint-setup/)&#x20;
* [Testing RPC connection](../testing-rpc-connection/)
* [API Reference](https://app.gitbook.com/s/FOeg95CadVyFvyLi70Bh/api-reference)
* [Limitless Node Homepage](https://getblock.io/limitless-node/)
