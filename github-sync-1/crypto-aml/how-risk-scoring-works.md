---
description: >-
  Learn how Crypto AML evaluates signals, assigns risk bands, and reports
  exposure.
---

# How Risk Scoring Works

Crypto AML evaluates on-chain and off-chain signals for each wallet. It returns a composite risk score, risk band, exposure breakdown, flags, and attribution.

### Scoring process

Each check passes through three stages.

#### 1. Build an evidence profile

The engine collects two types of evidence:

* **Address attribution:** whether the address is associated with a known owner, service, organization, or wallet cluster.
* **On-chain exposure:** the types of counterparties and activities connected to the address through observed transactions or asset flows.

A **signal** is a normalized piece of evidence—not merely a raw label. A signal may describe:

* a direct match to a reported or regulated entity;
* membership in an identified wallet cluster;
* exposure to a particular risk category;
* the direction of an interaction;
* the amount or share associated with that exposure;
* the strength and specificity of the available attribution.

Related internal findings may be combined into a broader customer-facing category. Internal source labels and provider-specific terminology are never returned in the public report.

#### 2. Calculate the risk score

The engine evaluates more than 30 criteria. Each signal is normalized and assigned an internal weight based on its relevance and evidential strength.

A simplified representation is:

```
signal contributionᵢ = weightᵢ × normalized signal valueᵢ
```

The engine then combines these contributions with rule-based checks and supervised machine-learning models:

```
overall risk score = scoring model(weighted signals, direct findings, attribution context)
```

This is more accurate than describing the result as a simple average. A direct, high-confidence finding may affect the result differently from indirect exposure, even when both appear in the same risk category.

The exact weights and decision rules are not exposed, but the report provides the evidence needed to understand the result. Median processing time is under two seconds.

#### 3. Return an interpretable report

The report includes:

* **Overall risk score:** the composite result from 0 to 100.
* **Exposure breakdown:** how the observed exposure is distributed across public categories.
* **Compliance flags:** direct findings that may require additional review.
* **Attribution:** identified owners, services, or wallet clusters, when available.
* **Calculation details:** the time of the assessment and its unique calculation ID.

The fields answer different questions:

* The **score** indicates the overall level of risk.
* The **exposure share** shows what the observed activity consists of; it is not the percentage contribution to the score.
* **Flags** identify direct or particularly significant findings.
* **Attribution** explains the entity or cluster context behind the address.

A customer should therefore read the score together with the exposure, flags, and attribution—not as a standalone decision.

### Signals evaluated

{% tabs %}
{% tab title="On-chain" %}
* On-chain behavioral patterns
* Wallet clusters
* Counterparty flow
* Smart contract exposure
* Direct & indirect exposure (hop distance to flagged addresses)
* Transaction volume & value concentration
* Peel-chain & layering patterns
* Mixer/tumbler interaction
* Bridge & cross-chain movement
* Wallet age & activity cadence
* Dormancy and sudden-reactivation signals
* Token-level exposure (per-asset flow scoring)
{% endtab %}

{% tab title="Off-chain" %}
* Sanctions feeds (OFAC, EU, UN, and national lists)
* Public attribution data
* Law-enforcement intelligence
* Darknet-market address databases
* Ransomware & extortion address feeds
* Scam, phishing & fraud reports
* Exchange & VASP attribution
* Known mixer/tumbler service lists
* High-risk jurisdiction indicators
* Reported hack & exploit address sets
{% endtab %}
{% endtabs %}

### Risk bands and thresholds

The composite `riskScore` ranges from `0` to `100`. It maps to one `riskBand`. The thresholds are exact:

| **Band (`riskBand`)** | **Label** | **Score range** | **Meaning**                                                                           |
| --------------------- | --------- | --------------- | ------------------------------------------------------------------------------------- |
| `low`                 | Low       | **0–24**        | No material risk indicators in the current evaluation.                                |
| `medium`              | Medium    | **25–49**       | Minor exposure detected. Reasonable to proceed after a quick review.                  |
| `high`                | High      | **50–74**       | Notable exposure to risky counterparties. Review details before accepting funds.      |
| `very-high`           | Very high | **75–100**      | Strong match with high-risk activity. Do not transact without enhanced due diligence. |

{% hint style="info" %}
**Scores are estimates, not definitive verdicts.** They combine weighted signals, machine-learning models, and updated data sources. So it is an estimate, not a definitive verdict. The same address can score slightly differently over time as on-chain activity, attribution data, and sanctions feeds change, and a result may sit near a band boundary. Treat the score and band as **one input to your own risk decision**, apply your own policy thresholds, and account for this natural fluctuation when acting on any particular result.

For **origin-of-funds** investigations, use our [**KYT**](#user-content-fn-1)[^1] **tool**. It traces movements between an address, its counterparties, and related addresses.
{% endhint %}

### Attribution

The report includes attribution when it can establish an identity:

* `owner` — a wallet owner
* `cluster` — a cluster identifier.
* `labels` — Entity names or inferred tags, such as `["sanctioned_entity", "mixer"]`.

[^1]: KYT stands for **Know Your Transaction**. It is an anti-money laundering (AML) and risk-mitigation process that tracks, analyzes, and evaluates financial transfers—especially in the cryptocurrency and fintech sectors—to detect illicit activities, sanctions exposure, and suspicious patterns in real time
