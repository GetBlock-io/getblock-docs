---
description: >-
  Crypto Address Audit is GetBlock's risk and compliance suite for screening
  blockchain addresses before any interaction.
---

# Crypto Address Audit: Risk & Compliance APIs

Crypto Address Audit is GetBlock's AI-powered risk and compliance suite for screening wallets and smart contracts — fraud risk, AML exposure, and rug pull detection in one API.

The service runs on pure on-chain behavioral analysis — no source code review, no off-chain data — across Ethereum, BNB Smart Chain, and Base.

### Choose Your Crypto Audit API:

1. **Wallet Audit**

A complete behavioral profile of a wallet address. Returns a trust score, AML check across 18+ parameters, behavioral risk profile (Risk Willingness, Experience Level, Risk Capability), protocol interaction history, transaction breakdown by category, and predictive intentions.

> Learn more about [Wallet Audit](https://getblock.io/wallet-audit-check/), check out it [docs](wallet-audit.md), or access its [endpoint](api-reference/wallet-audit-endpoint.md)

2. **Wallet Risk**

A faster, lighter check focused on a single binary outcome: is this address safe to interact with? Returns a risk score and the flags that drove it. Use this when latency matters and a full audit is more than the use case needs — payment flows, point-of-sale, real-time wallet connect.

> Learn more about [Wallet Risk](https://getblock.io/wallet-risk-check/), check out it [docs](wallet-risk.md), or access its [endpoint](api-reference/wallet-risk-endpoint.md)

3. **Rug Pull Checker**

A predictive check for smart contract and liquidity pool addresses. Analyzes the on-chain behavior of the contract creator and individual liquidity providers — tracing through intermediate contracts to the source wallet — and returns a rug pull risk score (0–100), risk level (LOW / MEDIUM / HIGH), creator trust score, per-LP trust scores, and behavioral flags. The underlying AI model is trained on 336K smart contracts.

> Learn more about [Rug Pull Checker](https://getblock.io/rug-pull-checker/), check out it [docs](rug-pull-checker.md), or access its [endpoint](api-reference/rug-pull-checker-endpoint.md)

### How to Access

Two ways to use Crypto Address Audit, depending on the workflow.

1. Via the [Dashboard](https://account.getblock.io/products/address-audit): This runs a check on the wallet/contract address through the UI. Paste an address, and run the check. **No code required.** Best for ad-hoc analysis, manual due diligence, and trying the service before integrating it.
2. [**Via the API** ](https://account.getblock.io/products/address-audit#api-keys)— one `POST` request per address, JSON response. Best for production integration into a DeFi protocol, wallet, launchpad, or any product that needs to screen addresses programmatically. [Access the available endpoints here](api-reference.md).

### Pricing & Limits

Crypto Address Audit uses a single subscription that covers all three services with a single API key.

Each user gets 5 free requests per day. After the free quota is used, each request costs $0.20 USD, deducted from the prepaid balance.

| Parameter         | Value           |
| ----------------- | --------------- |
| Free daily quota  | 5 requests      |
| Price per request | $0.20 USD       |
| Billing method    | Prepaid balance |

{% hint style="info" %}
**Need a custom setup** (higher rate limits, dedicated infrastructure, SLA, or volume pricing)? [Contact the GetBlock team](https://getblock.io/contact/).&#x20;
{% endhint %}

### Next Step

1. [Wallet Audit Doc](wallet-audit.md)
2. [Wallet Risk Doc](wallet-risk.md)
3. [Rug Pull Checker Doc](rug-pull-checker.md)
4. [Address Audit API Reference](api-reference.md)
5. [Wallet Audit Homepage](https://getblock.io/wallet-audit-check/)
6. [Wallet Risk Homepage](https://getblock.io/wallet-risk-check/)
7. [Rug Pull Checker Homepage](https://getblock.io/rug-pull-check/)
