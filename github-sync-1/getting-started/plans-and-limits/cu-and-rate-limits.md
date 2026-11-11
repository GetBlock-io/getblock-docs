---
description: >-
  GetBlock provides access to over 100 blockchains. CU and rate limits depend on
  the selected plan.
---

# CU and rate limits

This guide explains how limits work across all available plans, helping you understand what’s included and how to choose the option that best fits your current workload and future growth.

{% hint style="success" %}
* **Shared Nodes** operate on a system of limits defined by Compute Units (**CUs**) and Requests Per Second (**RPS**). Each plan also determines how many endpoints you can use simultaneously.
* **Limitless Node** has no CU limits - only an RPS limit.
* With **Dedicated Nodes**, you’re not limited by CUs or RPS.
{% endhint %}

***

### Shared node limits

GetBlock’s shared node service is subject to several usage limits. These are the key limits that directly affect costs and performance:

* [**CU (Compute Units)**](what-counts-as-a-cu.md): Measures the computational effort required to process requests. Different shared node plans include a varying number of CUs that you can use in a month.
* **RPS (Requests Per Second)**: Each plan enforces a maximum number of requests you can send every second. While you’re not billed per request, staying within this limit is critical to maintaining optimal service quality.
* [**Access Tokens**](../authentication-with-access-tokens.md)**:** Access tokens are unique identifiers used to authenticate your connection to GetBlock’s node infrastructure, generated when you create an endpoint. The limitation on your plan determines how many of these access tokens (and therefore endpoints) you can create.

<table data-search="false"><thead><tr><th>Plan</th><th width="129.24566650390625">Price (monthly)</th><th width="120.96441650390625">CU Allocation</th><th width="105.6171875">RPS Limit</th><th width="133.4599609375">Access Tokens (Endpoints)</th><th width="102.822021484375" data-type="checkbox">CU Top-ups</th></tr></thead><tbody><tr><td>Free</td><td>$0</td><td>50,000 / day</td><td>20 RPS</td><td>2</td><td>false</td></tr><tr><td>Starter</td><td>$49</td><td>90M / mo</td><td>250 RPS</td><td>30</td><td>true</td></tr><tr><td>Growth</td><td>$99</td><td>185M / mo</td><td>300 RPS</td><td>45</td><td>true</td></tr><tr><td>Advanced</td><td>$199</td><td>385M / mo</td><td>450 RPS</td><td>75</td><td>true</td></tr><tr><td>Scale</td><td>$349</td><td>700M / mo</td><td>600 RPS</td><td>105</td><td>true</td></tr><tr><td>Pro</td><td>$499</td><td>1B / mo</td><td>800 RPS</td><td>150</td><td>true</td></tr><tr><td>Premium</td><td>$699</td><td>1.6B / mo</td><td>800 RPS</td><td>225</td><td>true</td></tr><tr><td>Enterprise</td><td>from $999</td><td>Custom</td><td>Custom</td><td>Custom</td><td>true</td></tr></tbody></table>

Check [https://getblock.io/pricing/](https://getblock.io/pricing/) for current pricing and annual discounts across all tiers.

{% hint style="info" %}
Your **balance of CUs for Shared Nodes** is distributed on **all endpoints** added under the ‘Shared nodes’ tab.
{% endhint %}

{% tabs %}
{% tab title="Free" %}
The plan is ideal if you’re **just starting out** and do not have complex calls or large request volumes.

<figure><img src="../../.gitbook/assets/Free_widget_upd.svg" alt="GetBlock&#x27;s free RPC node plan limits"><figcaption></figcaption></figure>

* **CU**: 50,000/day
* **Throughput**: 20 requests per second (RPS)
* **Access Tokens**: 2

{% hint style="warning" %}
**Compute Units** are **renewed daily**, but unused CUs cannot be transferred to the next day.
{% endhint %}

To increase usage limits, choose between the higher-tier options.
{% endtab %}

{% tab title="Starter" %}
For use cases that are growing beyond the free tier — a first production app or side project. Offers a **significant increase in CU** and **RPS** compared to the Free plan.&#x20;

<figure><img src="../../.gitbook/assets/Starter_widget_jul26.svg" alt="Starter plan limits for Shared RPC nodes by GetBlock"><figcaption></figcaption></figure>

* **CU**: 90M per month (\~3M/day)
* **Throughput**: 250 requests per second (RPS)
* **Access Tokens**: 30
* Additional CU packages can be purchased as needed.
{% endtab %}

{% tab title="Growth" %}
For apps with a growing user base: higher daily call volumes and room for several services or environments at once.

<figure><img src="../../.gitbook/assets/Growth_widget_jul26.svg" alt="GetBlock &#x22;Growth&#x22; RPC plan limits"><figcaption></figcaption></figure>

* **CU**: 185M per month (\~6.2M/day)
* **Throughput**: 300 requests per second (RPS)
* **Access Tokens**: 45
* Additional CU packages can be purchased as needed.
{% endtab %}

{% tab title="Advanced" %}
**Mid-to-upper tier, production-ready** plan, suitable for moderate-to-high traffic applications.

<figure><img src="../../.gitbook/assets/Advanced_widget_jul26.svg" alt="GetBlock Advanced plan CU limits and RPS limits"><figcaption></figcaption></figure>

* **CU**: 385M per month (\~12.8M/day)
* **Throughput**: 450 requests per second (RPS)
* **Access Tokens**: 75
* Add extra compute units (CU) to your account balance when needed without switching plans
{% endtab %}

{% tab title="Scale" %}
For larger apps and teams. More CU headroom and endpoints to run many services or environments from one account.

<figure><img src="../../.gitbook/assets/Scale-widget-jul26.svg" alt="GetBlock&#x27;s &#x22;Scale&#x22; RPC node plan limits - Compute Units (CUs) and requests-per-second"><figcaption></figcaption></figure>

* **CU**: 700M per month (\~23.3M/day)
* **Throughput**: 600 requests per second (RPS)
* **Access** **Tokens**: 105
* Extra CU packages also available
{% endtab %}

{% tab title="Pro" %}
For applications that need significantly **higher throughput** and **increased resource availability** compared to lower tier plans.

<figure><img src="../../.gitbook/assets/Pro_widget_jul26.svg" alt="GetBlock&#x27;s &#x22;Pro&#x22; subscription plan CU allowance and rate limits"><figcaption></figcaption></figure>

* **CU**: 1B per month (\~33.3M/day)
* **Throughput**: 800 requests per second (RPS)
* **Access Tokens**: 150
* Purchase additional CU packages when required
{% endtab %}

{% tab title="Premium" %}
**Highest** standard tier before custom Enterprise terms. Built for sustained high-volume production traffic.

<figure><img src="../../.gitbook/assets/Premium_widget_jul26.svg" alt="GetBlock&#x27;s RPC plan with highest request and RPS limits - &#x22;Premium&#x22;"><figcaption></figcaption></figure>

* **CU**: 1.6B per month (\~53.3M/day)
* **Throughput**: 800 requests per second (RPS)
* **Access Tokens**: 225
* CU top-ups available
{% endtab %}

{% tab title="Enterprise" %}
**Fully customizable** with tailored CU allocations, rate limits, and access tokens to meet exceptionally high call volumes and performance requirements.

<figure><img src="../../.gitbook/assets/Enterprise_widget_upd.svg" alt="GetBlock&#x27;s Enterprise plan with custom usage limit settings"><figcaption></figcaption></figure>

* **CU**: Custom monthly allocation based on your demands
* **Throughput**: Custom
* **Access Tokens**: Custom
* Additional CU packages can be purchased on demand
{% endtab %}
{% endtabs %}

***

#### Managing unused & extra CUs

Your monthly plan CUs (main balance) are **valid for the current billing cycle** only. The balance **is reset** when your subscription renews. Main-balance CUs don't carry over.

Extra CUs (extra balance) — anything on top of your plan allocation — do **carry over to the next month**, as long as your subscription stays active and renews on time.

{% hint style="success" %}
If your demand exceeds the included limits, you can **purchase extra CU packages**. This means that even within a given plan, there’s room for scaling without an immediate need to move to a higher tier.

* [Top up CUs and boost limits](top-up-cus-and-boost-limits.md)
{% endhint %}

***

### Limitless node limits

Limitless Node removes Compute Unit limits entirely. You can send unlimited requests within the RPS tier you choose.

<figure><img src="../../.gitbook/assets/limitless_widget_2.webp" alt="GetBlock&#x27;s Limitless Node with fixed RPS and unlimited requests"><figcaption></figcaption></figure>

* **CU**: No limit&#x20;
* **Rate limit**: Capped at selected RPS tier

{% hint style="info" %}
See [Limitless Node](limitless-node.md) for tiers, supported chains, and setup.
{% endhint %}

***

### Dedicated node limits

Our Dedicated Node service is perfect for teams and projects that demand absolute freedom from rate limits and CU monitoring.

<figure><img src="../../.gitbook/assets/Dedic_widget.svg" alt="Dedicated node for scalable and unlimited RPC node access"><figcaption></figcaption></figure>

* **CU**: Unlimited
* **Rate limit**: Unlimited

***

If you’re unsure which plan best fits your needs, our team is ready to help! [Contact our support team](https://getblock.io/contact/) or visit our [Choosing your plan](choosing-your-plan.md) page for more information.
