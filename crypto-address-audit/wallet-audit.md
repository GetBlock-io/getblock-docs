---
description: >-
  GetBlock Wallet Audit: assess on-chain risk, AML exposure, and wallet behavior
  in seconds.
---

# Wallet Audit

Wallet audit is a service that conducts risk assessments for an automated wallet on the blockchain. The service analyzes on-chain wallet behavior and provides a comprehensive report that includes a trust score, an AML check, a behavioral profile, a protocol interaction history, and predictive intentions.

{% hint style="warning" %}
_Wallet Audit provides automated risk indicators based on publicly available on-chain data. The results are for informational purposes only and do not constitute AML compliance verification, legal advice, or regulatory screening. We do not guarantee the accuracy or completeness of the data. The decision to interact with an address is made by the user._
{% endhint %}

### Use Cases

Wallet Audit provides a complete behavioral wallet profile, not just a "fraud / not fraud" check. Below are six key scenarios where a full audit creates measurable value.

#### **1.  DeFi Protocol — Personalized Onboarding**

All users connecting to the protocol see the same interface, regardless of their experience, preferences, or financial capabilities. Beginners get lost in complex products, while experienced users don't see what they need.

With Wallet Audit, the moment a wallet connects, an audit runs instantly. Based on Risk Willingness, Experience, and Intentions, the protocol surfaces relevant products.&#x20;

**Example**:

* Beginner (Experience = 2, Risk Willingness = 3) → simple ETH staking, basic pools
* Experienced user (Experience = 10, Intentions: Leveraged Lending = HIGH) → directly to margin products, concentrated liquidity

> **Parameters used:** Experience Level, Risk Willingness, Intentions, Protocols

#### **2. Lending Protocol — Borrower Assessment**

In DeFi lending, all borrowers are assessed equally — only by collateral volume. There is no data on the borrower's financial stability or behavioral history.

With Wallet Audit, Risk Capability, Total Balance, and Experience, determine maximum loan size and collateral requirements.&#x20;

**Example**:

* Wallet A: Risk Capability = 1, balance $0.09, Experience = 10 → experienced but without funds. High collateral, low limit.
* Wallet B: Risk Capability = 8, balance $100K, Experience = 9 → experienced and financially stable. Standard collateral, increased limit.

> **Parameters used:** Risk Capability, Total Balance, Experience Level, Risk Willingness

#### **3. Growth/Marketing — User Base Segmentation**

Marketing campaigns in Web3 operate "blindly". The protocol only knows wallet addresses but doesn't understand who its users are, what they do, or what they need.

With Wallet Audit, a mass audit of connected wallets uses Transaction Categories, Protocols, and Intentions to split the base into cohorts for targeted marketing.&#x20;

Example:

* 40% of users with Prob\_Trade = HIGH and Uniswap interaction → launch a DEX aggregator for them
* 25% with Prob\_Lend = HIGH and Aave, Compound protocols → cross-promo with lending product
* 15% with Prob\_NFT = HIGH → partnership with NFT marketplace

> **Parameters used:** Transaction Categories, Protocols, Intentions, Experience Level

#### **4. Token Sale/Launchpad — Investor Quality Filtering**

Token sales are filled with airdrop farmers and bots who dump tokens on the first day of listing. Real investors don't receive allocation.

With Wallet Audit, Wallet Rank, Experience, and Risk Capability, provide objective applicant scoring. Allocation priority goes to wallets with a verified history.&#x20;

Example:

* Investor A: Wallet Rank in top 10K, Experience = 9, Risk Capability = 7, Intentions: Staking = HIGH → long-term holder, priority allocation
* Investor B: fresh wallet, 3 transactions, Wallet Rank = 0, Experience = 1 → likely farmer, low priority

> **Parameters used:** Wallet Rank, Experience Level, Risk Capability, Intentions, Transaction Count

#### **5. Quest Platforms — Adaptive Tasks and Sybil Protection**

Quest platforms (Layer3, Zealy, Galxe) offer the same tasks to everyone. Too difficult for beginners, too easy for experienced users. Bot farms collect rewards intended for real users.

With Wallet Audit, Experience and Protocols determine different quests for different levels, while Wallet Rank filters sybil accounts from real users.&#x20;

Example:

* Beginner (Experience = 2) → quest: "Make your first swap on Uniswap", "Stake ETH via Lido"
* Experienced (Experience = 8, Protocols: Aave, Lido, Curve) → quest: "Provide concentrated liquidity on Uniswap V3", "Create a leveraged position on Aave"
* Sybil filter: Wallet Rank < threshold or Transaction Count < 15 → wallet does not receive rewards

> **Parameters used:** Experience Level, Protocols, Wallet Rank, Transaction Count, Intentions

#### **6. Airdrop Campaigns — Farmer Filtering**

Most airdrop campaigns distribute tokens indiscriminately to everyone. Result — 80%+ of recipients sell on the first day, the price crashes, and real protocol users don't receive a fair allocation.

With Wallet Audit, Wallet Rank, Experience, and Transaction Categories objectively assess whether a wallet belongs to a real protocol user.

Example:

* Tier 1 (increased airdrop): Wallet Rank in top 20K, Experience >= 7, Transaction Categories include the protocol core category, AML = clean
* Tier 2 (standard airdrop): Wallet Rank in top 100K, Experience >= 4
* Exclusion: Wallet Rank = 0, Experience = 1, fewer than 15 transactions → airdrop farmer, does not receive allocation

> **Parameters used:** Wallet Rank, Experience Level, Transaction Categories, Predicted Trust, AML Analysis

{% hint style="danger" %}
### Limitations

* The service only works with regular wallets (EOA — Externally Owned Accounts). **Contract addresses are not supported.**
* A minimum of 10–15 transactions is required to calculate an accurate predictive score. Wallets with less history lack sufficient data for a reliable assessment
{% endhint %}

### Supported Network

1. Ethereum(ETH)
2. BNB Smart Chain(BSC)
3. Base

### How to Audit a Wallet Address&#x20;

1. Go to your [GetBlock Account Dashboard](https://account.getblock.io/products/address-audit#check)

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

2. Select the network you want to work with:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

3. Enter the wallet address you want to analyze:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

4. Click on **Run Check:**<br>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

5. Scroll down to see the analysis:

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>This report includes the following:</strong></summary>

1. Wallet balance, age, and number of transactions
2. Risk Willingness / Experience Level / Risk Capability scale

&#x20;         i. Risk Willingness: This measures psychological willingness to take risks. Determined by on-chain behavior: leverage usage, volatile assets, experimental protocols.

&#x20;          ii. Experience Level: This measures the depth and duration of Web3 activity: number of protocols, activity duration, transaction complexity, and multi-chain activity.

&#x20;           iii. Risk Capability: This measures financial ability to withstand losses, weighted by behavioral risk appetite.&#x20;

&#x20;      Roughly: available capital × Risk Willingness. A wallet with high willingness but a small balance gets a low Risk Capability — it can act riskily but cannot absorb meaningful loss.&#x20;

&#x20;       The reverse is also true: a large balance with low willingness scores low because the capital stays idle. ChainAware does not disclose the exact formula but considers asset size, diversification, and portfolio composition.&#x20;

3.  Predicted trust score: The main safety indicator — the probability that the wallet is legitimate. Displayed as a percentage from 0% to 100%.

    | Range     | Level       | Interpretation                                           |
    | --------- | ----------- | -------------------------------------------------------- |
    | 80–100%   | Low Risk    | Behavior typical of legitimate wallets                   |
    | 50–80%    | Medium Risk | Warning signals present, additional analysis recommended |
    | Below 50% | High Risk   | Behavioral patterns match known fraud schemes            |
4. Intentions: This predicts the next actions across categories, each receiving a score of HIGH, MEDIUM, or LOW, calculated based on the wallet's entire historical activity.
5. Recommendations: These show activities based on the wallet's risk profile. Examples: WBTC holding, ETH holding, Stablecoin lending.
6. Transaction: This shows the breakdown of historical transactions by type: DeFi, Decentralized Exchanges, Layer 1, Layer 2, NFT, Bridge, Lending & Borrowing, Business Services, Gaming. Shows the number of transactions in each category.
7. Protocols: This lists the specific protocols and services the wallet has interacted with, along with transaction counts. Examples: 1inch, Uniswap, Wrapped Ether, Tether, Zora, Arbitrum, Starknet, Opensea.
8. AML Analysis: This shows wallet check across 18+ parameters for links to criminal activity. Each parameter is displayed as Yes or No.

</details>

If you want to interact with or integrate this service via API, check the [Wallet audit endpoint.](api-reference/wallet-audit-endpoint.md)

{% hint style="info" %}
**Need a custom setup** (higher rate limits, dedicated infrastructure, SLA, or volume pricing)? [Contact the GetBlock team](https://getblock.io/contact/).&#x20;
{% endhint %}

### Next Step

1. [Wallet Risk Doc](wallet-risk.md)
2. [Rug Pull Checker Doc](https://app.gitbook.com/s/FOeg95CadVyFvyLi70Bh/crypto-address-audit)
3. [Address Audit API Reference](api-reference.md)
4. [Wallet Audit Homepage](https://getblock.io/wallet-audit-check/)
5. [Wallet Risk Homepage](https://getblock.io/wallet-risk-check/)
6. [Rug Pull Checker Homepage](https://getblock.io/rug-pull-check/)
