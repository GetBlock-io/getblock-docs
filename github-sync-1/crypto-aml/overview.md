---
description: >-
  GetBlock Crypto AML is a compliance-grade risk-screening service for crypto
  addresses and transactions
---

# Overview

GetBlock **Crypto** [**AML**](#user-content-fn-1)[^1] is a compliance-grade risk-screening service for crypto addresses and transactions. It evaluates wallet addresses or transaction hashes and sends a detailed structured risk report thats contains a composite score, an exposure breakdown, and FATF[^2]-aligned flags within seconds.

It is built to give small teams, exchanges, OTC[^3] desks, and individual traders the same FATF[^2]-aligned data and scoring as large compliance vendors, but **without** [**KYB**](#user-content-fn-4)[^4]**, annual contracts, or lengthy onboarding**.

{% hint style="info" %}
Access this service through your GetBlock account using a browser or the REST API. Charges use your existing credit balance.
{% endhint %}

### The problem it solves

Before you accept, send, or clear crypto funds, you will have to answer this question:

> _has this wallet been used by sanctioned entities, darknet markets, mixers, ransomware, or scams?_

Crypto AML answers that by tracing the address's on-chain history, wallet-to-wallet links, direct counterparty interactions, and public sanctions data — then condensing it into a decision-ready report.

Typical uses:

* **Screen a counterparty** before a transfer, while it is still reversible.
* **Safely accept incoming funds** so a downstream exchange doesn't flag or lock the deposit.
* **Onboard customers / KYT** — screen wallets during onboarding or ongoing monitoring.

### Supported networks

Crypto AML currently covers five networks — chosen because they carry the bulk of emerging-market exchange and remittance volume:

| Tag   | Network      |
| ----- | ------------ |
| `ETH` | Ethereum     |
| `TRX` | TRON         |
| `BTC` | Bitcoin      |
| `LTC` | Litecoin     |
| `BCH` | Bitcoin Cash |

{% hint style="info" %}
More networks are on the roadmap (Solana, Arbitrum, Optimism). The `currencyTag` used in the API path is the tag from the table above (e.g. `ETH`, `BTC`).
{% endhint %}

#### Sources of signals

The engine combines **on-chain** and **off-chain** data:

* **On-chain:** behavioral patterns, wallet clusters, counterparty flow, smart contract exposure, etc.
* **Off-chain:** sanctions feeds, public attribution data, law-enforcement intelligence.

{% hint style="info" %}
For the full model, thresholds, and how these combine into a score, see [**How risk scoring works**](how-risk-scoring-works.md).
{% endhint %}

### Access Crypto AML

Choose the access method that fits your workflow:

* **Web** — Select a network, enter a wallet address or transaction hash, and view the report. Export a PDF and keep an audit-ready history in your account.
* **REST API** — Integrate bearer-authenticated screening endpoints into your backend.

{% hint style="success" %}
Web and API checks use the same prepaid credit balance.
{% endhint %}

### Pricing

Choose a pricing model:

* **Pay as you go:** Top up your prepaid balance with a card or crypto. Each check deducts credits.
* **Bulk packages:** Buy checks upfront for a lower per-check rate. Larger packages receive larger discounts. Reach out to the [Sales team](mailto:support@getblock.io)

[^1]: Anti-Money Laundering (AML) consists of the laws, rules, and procedures used to stop criminals from hiding illegal "dirty" money as clean, legal income.

[^2]: The **Financial Action Task Force (FATF)** is an intergovernmental policy-making body founded in 1989 by the G7 to establish global standards for combating money laundering, terrorist financing, and proliferation financing.

[^3]: over-the-counter

[^4]: KYB (Know Your Business) is the process of verifying corporate entities, institutional clients, and merchant partners instead of individual retail users.
