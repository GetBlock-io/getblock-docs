# How risk scoring works

Crypto AML combines advanced analytics, machine-learning models and deterministic rules to evaluate dozens of on-chain and off-chain factors in parallel. The result is a structured breakdown across exposure axes — not a single opaque number.

### The pipeline

Every check runs through three stages:

1. **Signals — "what we read".** The engine reads 30+ criteria per wallet across two families of data.
2. **Scoring engine.** A weighted combination of every signal, mapped to a `0–100` range. Conceptually: `score = Σ wᵢ·sᵢ` over all signals `i`, blending **supervised ML models** with **heuristic rules**. Median latency is **< 400 ms**.
3. **Output — "what you get".** A composite score, a band, an exposure breakdown, FATF flags and attribution.

#### Signals evaluated

{% tabs %}
{% tab title="On-chain" %}
* On-chain behavioural patterns
* Wallet clusters
* Counterparty flow
* Smart contract exposure
* Direct & indirect exposure (hop distance to flagged addresses)
* Transaction volume & value concentration
* Peel-chain & layering patterns
* Mixer / tumbler interaction
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
* Known mixer / tumbler service lists
* High-risk jurisdiction indicators
* Reported hack & exploit address sets
{% endtab %}
{% endtabs %}

### Risk bands & thresholds

The `0–100` composite `riskScore` maps to one of four bands (`riskBand`). These thresholds are exact:

| Band (`riskBand`) | Label     | Score range | Meaning                                                                               |
| ----------------- | --------- | ----------- | ------------------------------------------------------------------------------------- |
| `low`             | Low       | **0–24**    | No material risk indicators in the current evaluation.                                |
| `medium`          | Medium    | **25–49**   | Minor exposure detected. Reasonable to proceed after a quick review.                  |
| `high`            | High      | **50–74**   | Notable exposure to risky counterparties. Review details before accepting funds.      |
| `very-high`       | Very high | **75–100**  | Strong match with high-risk activity. Do not transact without enhanced due diligence. |

{% hint style="info" %}
**The score is the output of a complex, probabilistic system.** It combines many weighted signals, machine-learning models and continuously updated data sources — so it is an estimate, not a definitive verdict. The same address can score slightly differently over time as on-chain activity, attribution data and sanctions feeds change, and a result may sit near a band boundary. Treat the score and band as **one input to your own risk decision**, apply your own policy thresholds, and account for this natural fluctuation when acting on any particular result.\
\
If you have concerns about the **origin of funds**, you can use our **KYT** tool to trace fund movements in a graph form — following an address to its counterparties and their interconnections — and decide based on that.&#x20;
{% endhint %}

### FATF flags

Alongside the score, each report lists discrete **FATF-aligned flags** you can cite in a case file. Values seen in the product include:

* `sanctions_list_match` — _Sanctions list match_
* `darknet_market_exposure` — _Darknet market exposure_
* `mixer_interaction` — _Mixer / tumbler interaction_
* _Ransomware-associated cluster_
* _Scam / phishing cluster_
* _High-risk jurisdiction_

{% hint style="info" %}
Flag values shown in `snake_case` are the API form; the italic labels are the UI display form.
{% endhint %}

### Exposure axes in the detailed report

The **detailed** view (API v2 / the UI report) scores risk along several axes so a clean present-day balance can't mask a risky history:

| Axis                        | What it measures                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Historical exposure**     | Aggregated risk across the wallet's entire on-chain history.                                            |
| **Current exposure**        | Risk weighted to the present-day balance and recent activity (last \~90 days).                          |
| **Direct interaction risk** | Exposure from counterparties one hop away — scored separately for incoming and outgoing flow.           |
| **Token-level breakdown**   | Each token on the wallet is scored independently — a clean ETH balance does not mask a risky USDT flow. |

### Attribution

Where identity can be established, the report includes attribution:

* `cluster` — a cluster identifier (e.g. `cluster_4f2a`).
* `labels` — entity names where known, inferred tags where not (e.g. `["sanctioned_entity", "mixer"]`).

