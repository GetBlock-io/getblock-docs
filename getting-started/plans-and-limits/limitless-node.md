---
description: >-
  A dedicated RPC endpoint with guaranteed RPS and no request limits, at a flat
  monthly price. For Ethereum, Solana, Bitcoin and other Tier-1 blockchains.
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
* **Full node** and **archive node** configurations for supported chains
* **Isolated capacity** and **dedicated routing** for a single network
* **Multi-region geo-routing** across Europe, the USA, and Asia to minimize latency through geographic proximity
* One instance can have **multiple** [**access tokens**](../authentication-with-access-tokens.md)**,** each a separate credential that can be regenerated or removed independently of the others
* **24/7 customer support** with a response time under 5 minutes
* **Pricing is fixed and predictable** because it is throughput-based rather than usage-based

{% hint style="success" %}
Limitless Node is a standalone product. You don't need an existing Shared or Dedicated plan to use it. You get a fully isolated RPC endpoint from day one.
{% endhint %}

***

### Supported chains

One Limitless Node subscription corresponds to one blockchain network. The service is currently available on 11 high-demand blockchains:

<table data-view="cards"><thead><tr><th></th></tr></thead><tbody><tr><td><img src="../../.gitbook/assets/Crypto Symbol=Ethereum.svg" alt="" data-size="line"> Ethereum</td></tr><tr><td><img src="../../.gitbook/assets/Logo=Solana.svg" alt="" data-size="line"> Solana</td></tr><tr><td><img src="../../.gitbook/assets/btc-logo.svg" alt="" data-size="line"> Bitcoin</td></tr><tr><td><img src="../../.gitbook/assets/base-logo.svg" alt="" data-size="line"> Base</td></tr><tr><td><img src="../../.gitbook/assets/bsc.svg" alt="" data-size="line"> BNB Smart Chain</td></tr><tr><td><img src="../../.gitbook/assets/Logo=Tron.svg" alt="" data-size="line"> Tron</td></tr><tr><td><img src="../../.gitbook/assets/pol-logo.svg" alt="" data-size="line"> Polygon</td></tr><tr><td><img src="../../.gitbook/assets/Logo=Avax.svg" alt="" data-size="line"> Avalanche</td></tr><tr><td><img src="../../.gitbook/assets/arb-logomark (1).svg" alt="" data-size="line"> Arbitrum</td></tr><tr><td><img src="../../.gitbook/assets/Logo=OP.svg" alt="" data-size="line"> Optimism</td></tr><tr><td><img src="../../.gitbook/assets/sui-logomark.svg" alt="" data-size="line"> Sui</td></tr></tbody></table>

{% hint style="info" %}
Additional networks may be added over time. If you need a chain that isn't listed yet, a custom RPS ceiling, or multi-region redundancy,[ contact our team](https://getblock.io/contact/).
{% endhint %}

***

### RPS tiers and pricing

Pick the tier that matches your current traffic. Within your RPS cap, requests are unlimited. There is no monthly request quota and no CU metering.

#### EVM Chains, Bitcoin, TRON, Sui - Full and Archive Nodes

{% tabs %}
{% tab title="Full Node" %}
| RPS Tier | Price (monthly) | Requests  |
| -------- | --------------- | --------- |
| 25 RPS   | $150/mo         | Unlimited |
| 50 RPS   | $300/mo         | Unlimited |
| 150 RPS  | $500/mo         | Unlimited |
| 300 RPS  | $1,000/mo       | Unlimited |

Supported networks: Arbitrum, Avalanche, Base, Bitcoin, BNB Smart Chain, Ethereum, Optimism, Polygon, Sui, TRON
{% endtab %}

{% tab title="Archive Node" %}
| RPS Tier | Price (monthly) | Requests  |
| -------- | --------------- | --------- |
| 15 RPS   | $150/mo         | Unlimited |
| 30 RPS   | $300/mo         | Unlimited |
| 50 RPS   | $500/mo         | Unlimited |
| 100 RPS  | $1,000/mo       | Unlimited |

Supported Networks: Arbitrum, Base, BNB Smart Chain, Ethereum, Optimism, Polygon, Sui, TRON
{% endtab %}
{% endtabs %}

#### Solana - Full Node

| RPS Tier | Price (monthly) | Request   |
| -------- | --------------- | --------- |
| 5 RPS    | $150/mo         | Unlimited |
| 15 RPS   | $450/mo         | Unlimited |
| 30 RPS   | $900/mo         | Unlimited |
| 50 RPS   | $1,500/mo       | Unlimited |

Prices shown are for monthly billing. Annual billing saves 20%. You can move between tiers at any time with no lock-in.

{% hint style="warning" %}
Requests above your RPS limit are rate-limited and return a standard rate-limit response. **We do not charge overage fees**. If you consistently reach your limit, consider upgrading to a higher tier.
{% endhint %}

***

### Deploy a Limitless Node

You can configure and launch a Limitless Node from your GetBlock **Dashboard** → **Limitless Nodes.**

1. In your account, open Limitless Nodes and click Create new node.
2. Configure the node:
   1. The blockchain network
   2. Mode - Full or Archiv&#x65;_\*_
   3. Node location
   4. API interface (all supported interfaces included by default)
3. Select an RPS plan
4. Choose a billing term: 1, 6, or 12 months
5. Complete checkout

_\*Not every network offers an archive tier. Check availability in your dashboard first._

If you need a chain that isn't listed, [contact our team](https://getblock.io/contact/) directly. We can also recommend a tier based on your workload.

***

#### Next steps

* [Endpoint setup](../endpoint-setup/)
* [Testing RPC connection](../testing-rpc-connection/)
* [API Reference](https://app.gitbook.com/s/FOeg95CadVyFvyLi70Bh/api-reference)
* [Limitless Node Homepage](https://getblock.io/limitless-node/)
