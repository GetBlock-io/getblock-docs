---
description: >-
  GetBlock provides access to over 50 blockchains. Request and rate limits
  depend on the selected plan.
---

# Request and rate limits

<figure><img src="../../.gitbook/assets/Docs_Plan_Comparison.svg" alt="Comparison table of GetBlock&#x27;s shared vs dedicated nodes"><figcaption><p>Feature comparison of shared nodes vs dedicated nodes</p></figcaption></figure>

To see the full comparison table, navigate to [https://getblock.io/pricing/](https://getblock.io/pricing/).

***

### Shared node limits

<figure><img src="../../.gitbook/assets/Docs_shared_limits(new).svg" alt="Shared RPC node plans and options"><figcaption><p>Shared node plans</p></figcaption></figure>

#### 1) Free plan

* Requests: 5,000 per day per account.
* Rate: 5 requests per second (RPS).

{% hint style="info" %}
Requests are renewed daily, but unused requests cannot be transferred to the next day.
{% endhint %}

Clients who want to increase their usage limits can choose between the options below.

#### 2) Start plan

This is a monthly subscription based on the number of requests you need.

* Requests: from 5 to 100 million per month.
* Rate: 200 requests per second (RPS).
* You can buy extra request packages if needed.

{% hint style="info" %}
If you don’t use all your allocated requests within a month, the unused requests will carry over to the next month as long as your subscription is active and renewed.
{% endhint %}

<figure><img src="../../.gitbook/assets/Shared_limits_Start.jpg" alt="Start RPC node plan limits"><figcaption><p>Start plan user dashboard</p></figcaption></figure>

> If Alice subscribes to a one-month plan with 50 million requests starting on November 10, the plan will expire on December 10. Any unused requests will be available again only after she renews her subscription for the next period.
>
> Additionally, if Alice finds that 50 million requests are insufficient for her needs, she can purchase additional request packages directly from her dashboard.

Here is the calculation that helps to determine the time during which the whole amount of requests from the “Start” subscription will be spent at a maximum capacity set to 200 requests per second (RpS):

* 5,000,000 requests / (200 RpS \* 60 sec) = 416 min (or 6 hrs 57 min);
* 10,000,000 requests / (200 RpS \* 60 sec) = 833 min (or 13 hrs 53 min);
* 50,000,000 requests / (200 RpS \* 60 sec) = 4166 min (or 69 hrs 27 min);
* 100,000,000 requests / (200 RpS \* 60 sec) = 8332 min (or 138 hrs 54 min).

{% hint style="info" %}
Your balance of requests for shared nodes is distributed on all endpoints added under the ‘Shared nodes’ tab.
{% endhint %}

#### 3) Unlimited plan

<figure><img src="../../.gitbook/assets/Docs_unlim_dashboard.jpg" alt="RPC node with unlimited access"><figcaption><p>Unlimited plan user dashboard</p></figcaption></figure>

This is a monthly subscription with no limit on the number of requests on all shared nodes for 1, 3, 6, 9, or 12 months.

* Requests: unlimited.
* Default rate: 300 requests per second (RPS).

***

### Dedicated node limits

<figure><img src="../../.gitbook/assets/Docs_dedic_limits.jpg" alt="Dedicated node for scalable and unlimited RPC node access"><figcaption><p>Dedicated node features</p></figcaption></figure>

* Requests: unlimited
* Rate: unlimited (1000+ requests/sec depending on a blockchain).
