---
description: >-
  Learn about GetBlock Rug Pull Checker, its benefits and how to use to analyze
  contract address
---

# Rug Pull Checker

Rug Pull Checks is a smart-contract rug-pull risk assessment service. The service analyzes contracts across two independent dimensions and returns an overall risk score.

Two independent analysis blocks:

1. Rug Pull Probability: AI-predictive score (0–100%). Analyzes the behavior of the contract creator and liquidity providers (LPs) based on their on-chain history. Does not analyze contract code.
2. Contract Details: verification of basic contract properties (open source, proxy, self-destruct, withdrawal rights, blacklist). Does not affect the Rug Pull Probability.

{% hint style="warning" %}
These are two independent blocks. Contract Details (green checkmarks) do not affect the Rug Pull Probability. A contract with clean code, but a suspicious creator will receive a high risk score.
{% endhint %}

### Difference Between Rug Pull Checker And Wallet Audit

| <p><br></p>      | **Rug Pull Checks**                                | **Wallet Audit**                       |
| ---------------- | -------------------------------------------------- | -------------------------------------- |
| Input            | Smart contract address                             | Wallet address (EOA)                   |
| What it analyzes | Creator behavior + LPs + basic contract properties | On-chain wallet behavior, AML, intent  |
| Primary score    | Rug Pull Probability (0–100%)                      | Predicted Trust (0–100%)               |
| When to use      | Before investing in a contract/pool/token          | Before transacting with a counterparty |

### Difference Between Rug Pull Checker And Risk API (Hexens Glider)

GetBlock also provides the Risk API service, powered by Hexens Glider Token Risks. This is a fundamentally different product; they do not compete but complement each other.

| <p><br></p>      | **Rug Pull Checks**                                            | **Risk API**                                                                                                    |
| ---------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Provider         | ChainAware.ai                                                  | Hexens (Glider Token Risks)                                                                                     |
| What it analyzes | Behavior of the contract creator and liquidity providers (LPs) | Smart contract code — logic of each function and dependencies                                                   |
| Method           | Behavioral AI/ML — analysis of on-chain wallet history         | Static code analysis — parsing of ERC-20 function logic                                                         |
| Key question     | "Can the people behind this contract be trusted?"              | "Is there malicious code in this contract?"                                                                     |
| What it catches  | Suspicious creators, new LP addresses, links to fraud wallets  | Honeypot, hidden mint, blacklist, selfdestruct, hidden fees, backdoors, pausable proxy, etc. (22+ threat types) |
| Networks         | ETH, BNB, Base                                                 | 32+ EVM chains                                                                                                  |
| Analogy          | Catches the "bomber by handwriting"                            | Catches the "bomb in the code"                                                                                  |

{% hint style="info" %}
Both products complement each other: ChainAware may give a green light to a contract with a clean deployer, but with a honeypot in the code. Hexens may show clean code, but the contract was created by a wallet that previously executed a rug pull. Using the two gives a full picture.
{% endhint %}

{% hint style="warning" %}
Disclaimer

Rug Pull Check provides automated risk indicators based on publicly available on-chain data. Results are informational in nature and do not constitute investment advice, a security audit, or legal counsel. The predictive model accuracy is 68% — 32% of rug pulls may go undetected. We do not guarantee the accuracy or completeness of the data. The decision to interact with a contract is made by the user.
{% endhint %}

### Supported Networks

| **Network**     | **API Value** | **Note**     |
| --------------- | ------------- | ------------ |
| Ethereum        | ETH           | Full support |
| BNB Smart Chain | BNB           | Full support |
| Base            | BASE          | Full support |

### How Rug Pull Probability Is Calculated&#x20;

There are four steps involved, which are:

1. Find the contract creator: It identifies the wallet that deployed the contract and runs it through the Fraud Detector to obtain the creator's Trust Score.
2. Trace the deployment chain: if the contract was deployed by another contract, it follows the chain to the actual wallet. **The obfuscation through the chain itself is a red flag.**
3. Analyze the liquidity providers (LPs): run each LP through the Fraud Detector and obtain the Trust Score for each LP.
4. Generate the Rug Pull Probability: based on the combination of the creator's Trust Score + LP Trust Scores.

#### What increases the risk score:

* New creator address (no history to evaluate)
* Low Trust Score for the creator (behavioral history matches fraud patterns)
* New addresses adding liquidity (classic rug pull pattern)
* Low Trust Score for LPs
* Obfuscated deployment chain (contract deploys contract)

#### What decreases the risk score:

* Creator with a long clean on-chain history
* LPs with high Trust Scores
* Transparent addresses without routing through mixers

> Accuracy: 68%

The algorithm correctly identifies 68 out of 100 rug pulls based on purely behavioral analysis, without code analysis. The 32% miss rate consists of more sophisticated operators who invest in building a legitimate-looking wallet history before executing a rug pull.

{% hint style="danger" %}
#### Limitations

* The service works only with smart contracts. If a regular wallet address (EOA) is submitted, an empty result will be returned. To check wallets, use Wallet Audit.
* Rug Pull Probability does not analyze the contract source code. It evaluates the behavior of the people behind the contract, not the code.
* Data freshness depends on the last ChainAware scan (lastChecked field).
{% endhint %}

### How to Check the Risk In A Wallet Address

1. Go to your [GetBlock Account Dashboard](https://account.getblock.io/products/address-audit#check) and click **Rug Pull**

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

2. Select the choice of your network using the dropdown:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

3. Enter the contract address:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

4. Click on **Run Check**
5. Scroll down to see the analysis:

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>This report includes the following:</strong></summary>

1. Contract name
2. Contract address
3. Creator wallet address
4. Network
5. Badges
6. Rug Pull probability: The primary indicator, ranging from 0% to 100%, is displayed with a progress bar and a High Risk / Medium Risk / Low Risk label.
7. Contract details: This contains parameters that are displayed as a green checkmark (safe) or a red flag (risk).

</details>

### Next Step

* [Address Audit API Reference](api-reference.md)
