---
description: >-
  GetBlock Crypto AML is a compliance-grade risk-screening service for crypto
  addresses and transactions
---

# Overview

GetBlock **Crypto AML** is a compliance-grade risk-screening service for crypto addresses and transactions. Paste a wallet or a transaction hash, and get back a structured risk report — a composite score, an exposure breakdown, and FATF-aligned flags — in seconds.

It is built to give small teams, exchanges, OTC desks and individual traders the same FATF-aligned data and scoring that large compliance vendors offer, but **without KYB, annual contracts, or lengthy onboarding**. The same account and the same credit balance power both the browser UI and the REST API.

### The problem it solves

Before you accept, send, or clear crypto funds, you often need to know: _has this wallet touched sanctioned entities, darknet markets, mixers, ransomware, or scams?_ Crypto AML answers that by tracing the address's on-chain history, wallet-to-wallet links, direct counterparty interactions, and public sanctions data — then condensing it into a decision-ready report.

Typical uses:

* **Screen a counterparty** before a transfer, while it is still reversible.
* **Accept incoming funds safely** so a downstream exchange doesn't flag or lock the deposit.
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

* **On-chain** — behavioural patterns, wallet clusters, counterparty flow, smart contract exposure etc.
* **Off-chain** — sanctions feeds, public attribution data, law-enforcement intelligence.

For the full model, thresholds, and how these combine into a score, see [**How risk scoring works**](how-risk-scoring-works.md).

### How to use?

Crypto AML exposes the data through two paths:

* **Web** — a convenient browser flow: pick a network, paste a wallet or transaction, read the report, export a PDF, and keep an audit-ready history under your account.&#x20;
* **REST API** — three bearer-authenticated endpoints for developers who want to wire screening into their own backend.&#x20;

{% hint style="success" %}
Prepaid credits are burned in the UI and via the API draw from the same balance.
{% endhint %}

### Pricing

Choose the model that fits how you work. With **Pay-as-you-go**, you top up a prepaid balance and we deduct it per check — starting from \~$0.20 / check, with no subscriptions and no minimums. Top up anytime with a card or crypto. \
\
Prefer to buy in bulk? Our let you purchase packages of checks upfront and pay a lower per-check rate — the larger the package, the bigger the discount.&#x20;

