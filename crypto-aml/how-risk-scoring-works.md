---
description: >-
  Learn how Crypto AML evaluates signals, assigns risk bands, and reports
  exposure.
---

# How Risk Scoring Works

Crypto AML evaluates on-chain and off-chain signals for each wallet. It returns a composite risk score, risk band, exposure breakdown, flags, and attribution.

### Scoring process

Each check has three stages:

1. **Signals — "what we read":** The engine evaluates more than 30 criteria across two data families.
2. **Scoring engine:** Weighted signals produce a score from `0` to `100`. The engine combines supervised machine-learning models with heuristic rules. Median latency is under 400 ms.
3. **Return the report:** The report includes a composite score, band, exposure breakdown, FATF[^1]-aligned flags, and attribution.

{% hint style="info" %}
Conceptually, the scoring calculation is `score = Σ wᵢ·sᵢ`. Each `wᵢ` is a signal weight. Each `sᵢ` is a signal value.
{% endhint %}

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

For **origin-of-funds** investigations, use our [**KYT**](#user-content-fn-2)[^2] **tool**. It traces movements between an address, its counterparties, and related addresses.
{% endhint %}

### FATF flags

Each report includes discrete FATF[^1]-aligned flags for case records. The product can return these flags:

* `sanctions_list_match` — _Sanctions list match_
* `darknet_market_exposure` — _Darknet market exposure_
* `mixer_interaction` — _Mixer/tumbler interaction_
* _Ransomware-associated cluster_, _Scam/phishing cluster_, and _High-risk jurisdiction_

{% hint style="info" %}
Values in `snake_case` are API values. Italic text is the UI label.
{% endhint %}

### Risk Categories

<table data-search="false"><thead><tr><th width="231.03125">API category</th><th width="157.353515625">Public label</th><th>Group</th><th width="226.708984375">Customer-facing meaning</th></tr></thead><tbody><tr><td><code>cyberThreatActivity</code></td><td>Cyber threat activity</td><td>Risk</td><td>Activity linked to hacking, phishing, malicious software, stolen credentials, compromised wallets or accounts, and attacks against blockchain protocols or digital services.</td></tr><tr><td><code>extortionActivity</code></td><td>Extortion activity</td><td>Risk</td><td>Activity involving ransomware-related extortion, ransom demands, blackmail, or other coercive attempts to obtain assets.</td></tr><tr><td><code>fraudulentSchemes</code></td><td>Fraudulent schemes</td><td>Risk</td><td>Deceptive schemes intended to obtain funds or assets, including scams and fraudulent investment structures.</td></tr><tr><td><code>stolenAssets</code></td><td>Stolen assets</td><td>Risk</td><td>Funds or other digital assets reported as stolen, misappropriated, or connected to theft.</td></tr><tr><td><code>darknetActivity</code></td><td>Darknet activity</td><td>Risk</td><td>Activity associated with darknet marketplaces or services used for illegal purposes.</td></tr><tr><td><code>illegalGoodsTrafficking</code></td><td>Illegal goods trafficking</td><td>Risk</td><td>Activity associated with the production, sale, or distribution of prohibited goods, including controlled drugs or weapons.</td></tr><tr><td><code>childAbuse</code></td><td>Child abuse</td><td>Risk</td><td>Activity associated with the abuse or exploitation of children.</td></tr><tr><td><code>humanExploitation</code></td><td>Human exploitation</td><td>Risk</td><td>Activity associated with human trafficking, forced exploitation, or the unlawful trade of people.</td></tr><tr><td><code>terrorismAndExtremism</code></td><td>Terrorism and extremism</td><td>Risk</td><td>Activity associated with terrorist or extremist organizations, their operations, support, or financing.</td></tr><tr><td><code>proliferationFinancing</code></td><td>Proliferation financing</td><td>Risk</td><td>Activity associated with financing the proliferation of weapons of mass destruction.</td></tr><tr><td><code>transactionObfuscation</code></td><td>Transaction obfuscation</td><td>Risk</td><td>Use of services or techniques intended to conceal transaction origins, destinations, ownership, or movement of funds.</td></tr><tr><td><code>gambling</code></td><td>Gambling</td><td>Risk</td><td>Activity associated with gambling services.</td></tr><tr><td><code>otherCriminalActivity</code></td><td>Other criminal activity</td><td>Risk</td><td>Criminal activity that cannot be represented by a more specific public risk category.</td></tr><tr><td><code>sanctions</code></td><td>Sanctions</td><td>Compliance</td><td>A reported connection to an official sanctions designation or a jurisdiction subject to sanctions restrictions.</td></tr><tr><td><code>jurisdictionRisk</code></td><td>Jurisdiction risk</td><td>Compliance</td><td>Exposure associated with a jurisdiction identified as presenting elevated AML or counter-terrorist-financing risk.</td></tr><tr><td><code>limitedComplianceControls</code></td><td>Limited compliance controls</td><td>Compliance</td><td>Exposure to a service with weak, missing, or insufficient identity verification and compliance controls.</td></tr><tr><td><code>legalAndRegulatoryAttention</code></td><td>Legal and regulatory attention</td><td>Compliance</td><td>A signal requiring enhanced review because of regulatory, legal, enforcement, political-exposure, litigation, or asset-seizure context.</td></tr><tr><td><code>otherComplianceRisk</code></td><td>Other compliance risk</td><td>Compliance</td><td>A compliance-related concern that does not fit a more specific public category.</td></tr><tr><td><code>insufficientInformation</code></td><td>Insufficient information</td><td>Compliance</td><td>Available information is not sufficient to assign a meaningful risk or entity category.</td></tr><tr><td><code>assetRestriction</code></td><td>Asset restriction</td><td>Compliance</td><td>A specific asset associated with the address has been restricted or blocked by its issuer or another responsible authority.</td></tr><tr><td><code>assetTradingService</code></td><td>Asset trading service</td><td>Entity / service</td><td>A service used to buy, sell, or exchange digital assets, including direct and brokered trading models.</td></tr><tr><td><code>decentralizedFinance</code></td><td>Decentralized finance</td><td>Entity / service</td><td>A decentralized protocol or application providing trading, lending, staking, or other financial functionality.</td></tr><tr><td><code>walletAndCustody</code></td><td>Wallet and custody</td><td>Entity / service</td><td>A wallet or custody service that stores, manages, or safeguards digital assets.</td></tr><tr><td><code>paymentsAndCommerce</code></td><td>Payments and commerce</td><td>Entity / service</td><td>A service facilitating cryptocurrency payments, purchases, or commercial transactions.</td></tr><tr><td><code>fundraisingAndInvestment</code></td><td>Fundraising and investment</td><td>Entity / service</td><td>Activity involving investment products, fundraising, donations, crowdfunding, or charitable funding.</td></tr><tr><td><code>miningActivity</code></td><td>Mining activity</td><td>Entity / service</td><td>Activity associated with cryptocurrency mining, mining pools, or remotely provided mining services.</td></tr><tr><td><code>onchainApplication</code></td><td>On-chain application</td><td>Entity / service</td><td>A blockchain application or smart-contract-based service, including games and digital-asset applications.</td></tr><tr><td><code>technologyInfrastructure</code></td><td>Technology infrastructure</td><td>Entity / service</td><td>Technical infrastructure or hosting used to support blockchain or digital-asset services.</td></tr><tr><td><code>institutionalEntity</code></td><td>Institutional entity</td><td>Entity / service</td><td>A government, political, educational, cultural, or media organization.</td></tr><tr><td><code>personalWallet</code></td><td>Personal wallet</td><td>Entity / service</td><td>An address attributed to an individual rather than an identified service or organization.</td></tr><tr><td><code>adultContentService</code></td><td>Adult content service</td><td>Entity / service</td><td>A service associated with adult content or related commercial activity.</td></tr></tbody></table>

### Exposure in the detailed report

The detailed API v2 and UI reports score several exposure axes. This prevents a clean balance from masking historical risk:

<table><thead><tr><th width="206.55078125">Axis</th><th>What it measures</th></tr></thead><tbody><tr><td><strong>Historical exposure</strong></td><td>Aggregated risk across the wallet's entire on-chain history.</td></tr><tr><td><strong>Current exposure</strong></td><td>Risk weighted to the current balance and recent activity from about 90 days.</td></tr><tr><td><strong>Direct interaction risk</strong></td><td>Exposure from counterparties one hop away — scored separately for incoming and outgoing flow.</td></tr><tr><td><strong>Token-level breakdown</strong></td><td>Each token on the wallet is scored independently — a clean ETH balance does not mask a risky USDT flow.</td></tr></tbody></table>

### Attribution

The report includes attribution when it can establish an identity:

* `cluster` — A cluster identifier, such as `cluster_4f2a`.
* `labels` — Entity names or inferred tags, such as `["sanctioned_entity", "mixer"]`.

[^1]: The **Financial Action Task Force (FATF)** is an intergovernmental policy-making body founded in 1989 by the G7 to establish global standards for combating money laundering, terrorist financing, and proliferation financing.

[^2]: KYT stands for **Know Your Transaction**. It is an anti-money laundering (AML) and risk-mitigation process that tracks, analyzes, and evaluates financial transfers—especially in the cryptocurrency and fintech sectors—to detect illicit activities, sanctions exposure, and suspicious patterns in real time
