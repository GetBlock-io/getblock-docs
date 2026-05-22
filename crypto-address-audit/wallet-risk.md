---
description: >-
  GetBlock Wallet Risk: fast AI fraud screening for wallets — risk score, level,
  and flags.
---

# Wallet Risk

Wallet Risk Check is a quick blockchain wallet risk assessment service. It returns an AI-predictive trust score, screening across 18 AML risk categories, and a sanctions check — in a single API call with response time under 100ms.

This is a lightweight version of [Wallet Audit](wallet-audit.md). Wallet Risk Check provides a quick go/no-go signal e.g "Can this wallet be trusted?"

{% hint style="info" %}
For a full behavioral profile (intentions, experience, protocols, transactions), use [Wallet Audit.](wallet-audit.md)
{% endhint %}

### What Wallet Risk Does?

* Predicted Trust Score (0–100%): AI-predictive trust score
* AML Risk Screening: screening across 18 risk categories (cybercrime, money laundering, phishing, etc.)
* Sanctions Check: verification against sanctions lists

### Difference Between Wallet Risk And Wallet Audit

| <p><br></p>                   | Wallet Risk Check                 | Wallet Audit                      |
| ----------------------------- | --------------------------------- | --------------------------------- |
| Response time                 | < 100ms                           | Several seconds                   |
| Trust Score                   | Yes                               | Yes                               |
| AML Screening (18 categories) | Yes                               | Yes                               |
| Sanctions Check               | Yes                               | Yes                               |
| Intentions (14 categories)    | No                                | Yes                               |
| Experience / Risk Profile     |  No                               | Yes                               |
| Protocols / Categories        | No                                | Yes                               |
| Wallet Overview / Rank        | No                                | Yes                               |
| Networks                      | 5 (ETH, BNB, Base, Polygon, TRON) | 3 (ETH, BNB, Base)                |
| When to use                   | Quick screening: allow / reject   | Deep analysis: who is this wallet |

### How Predicted Trust Score is Calculated

They are two-step logic involved in this calculation:

#### Step 1: AML Check (hard override)

If at least one field in forensic\_details = "1" → probabilityFraud is automatically set to 1.0 (Predicted Trust = 0%). The ML model is not invoked. Any AML flag = automatic maximum risk.

#### Step 2: ML Model (if AML is clean)

If all forensic\_details fields = "0" → the predictive AI model analyzes on-chain wallet behavior and returns probabilityFraud from 0.0 to 1.0. Predicted Trust = 1 − probabilityFraud.

Examples:

* vitalik.eth: all forensic\_details = "0" → ML model → probabilityFraud = 0.042 → Predicted Trust = 95.8%
* Fraudulent wallet: money\_laundering = "1" → hard override → probabilityFraud = 1.0 → Predicted Trust = 0%

{% hint style="danger" %}
### Limitations

* The service only works with regular wallets (EOA — Externally Owned Accounts). **Contract addresses are not supported.**
* A minimum of 10–15 transactions is required to calculate an accurate predictive score. Wallets with less history lack sufficient data for a reliable assessment
{% endhint %}

### Supported Networks

1. Ethereum(ETH)
2. BNB Smart Chain
3. Base&#x20;
4. Polygon
5. TRON

### How to Check the Risk In A Wallet Address

1. Go to your [GetBlock Account Dashboard](https://account.getblock.io/products/address-audit#check) and click **Wallet Risk**

<figure><img src="../.gitbook/assets/image (21).png" alt="wallet risk"><figcaption></figcaption></figure>

2. Select the choice of your network using the dropdown:

<figure><img src="../.gitbook/assets/image (22).png" alt="network selection"><figcaption></figcaption></figure>

3. Enter the wallet address:

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

4. Click on **Run Check:**

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

5. Scroll down to see the analysis:

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>This report includes the following:</strong></summary>

1. Predicted trust score: This is the main indicator of a wallet's level of trust, ranging from 0% to 100%. Calculated as 1 − probability of fraud. Displayed with a status badge (Not Fraud / Fraud / New Address) and a sanctions badge (Not Sanctioned / Sanctioned).

| Predicted Trust | Level       | Interpretation                                                 |
| --------------- | ----------- | -------------------------------------------------------------- |
| 80–100%         | Low Risk    | Wallet with clean history                                      |
| 50–79%          | Medium Risk | Risk factors detected, full Wallet Audit recommended           |
| 0–49%           | High Risk   | Wallet is highly likely associated with fraudulent activity    |
| 0% (auto)       | AML Flag    | At least one AML flag detected — score automatically set to 0% |

2. AML Analysis: This shows 18 risk categories with each parameter displayed as  No (clean) or Yes (flag detected).

{% hint style="warning" %}
_If at least one parameter forensic\_details = "1" → probabilityFraud is automatically set to 1.0 (i.e. Predicted Trust = 0%). This is a hard override; the ML model is not used._
{% endhint %}

3. Sanctions Check: This shows sanctions list verification. It displayed as a badge: Not Sanctioned (green) or Sanctioned (red). If the wallet is sanctioned, the category, name, and source link are displayed.

</details>

If you want to interact with or integrate this service via API, check the [Wallet risk endpoint.](api-reference/wallet-risk-endpoint.md)

{% hint style="info" %}
**Need a custom setup** (higher rate limits, dedicated infrastructure, SLA, or volume pricing)? [Contact the GetBlock team](https://getblock.io/contact/).&#x20;
{% endhint %}

### Next Step

1. [Wallet Audit](wallet-audit.md)
2. [Rug Pull Checker](/broken/pages/5p9srabFofhIIsU5dubn)
3. [Address Audit API Reference](api-reference.md)
