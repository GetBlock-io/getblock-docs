---
hidden: true
---

# Run a check in the dashboard

The web UI is the fastest way to screen a wallet or transaction — no code required.

{% hint style="info" %}
Getting started takes three things: sign up with an email, top up balance, and start screening. **No KYB and no annual contract.**
{% endhint %}

Entry point: [https://account.getblock.io/products/aml-check](https://account.getblock.io/products)→ **Run a check**.

### Screening in three steps

The individual flow is a simple, three-step screen:

<table><thead><tr><th width="86.8984375">Step</th><th>Action</th></tr></thead><tbody><tr><td><strong>01</strong></td><td><strong>Pick a network</strong> — <code>ETH</code>, <code>TRX</code> , <code>BTC</code>, <code>LTC</code>, <code>BCH</code></td></tr><tr><td><strong>02</strong></td><td><strong>Paste a wallet or transaction</strong> — an address or a transaction hash.</td></tr><tr><td><strong>03</strong></td><td><strong>Read the report</strong> — a risk score, exposure breakdown, and FATF flags in seconds.</td></tr></tbody></table>

#### Reading the result screen

The report ("**Address risk — Detailed view**") surfaces:

* **Total risk score** — the wallet-wide `0–100` result. It reflects the wallet’s complete screened history across the returned assets, not only its current balance or riskiest token. The interface groups it as **Low** (`0–24`), **Medium** (`25–49`), **High** (`50–74`), or **Very high** (`75–100`).
* **Recommendation** — a plain-language interpretation and suggested review level based on the score. It highlights what to examine but does not automatically tell you to accept or reject the wallet.
* **Owner** — the known entity believed to control the address, such as an exchange or service. An owner is an identity signal and does **not** have its own risk score.
* **Cluster** — a group of addresses linked through common control or on-chain behaviour. Unlike an owner, a cluster has its own risk score and can raise the wallet’s total score even when the address has little history or no risky current balance.
* **Labels** — external-list memberships associated with the address, such as an issuer freeze, sanctions reference, or functional role such as contract deployer. Critical labels are highlighted, but a label is not automatically a risk score or proof of wrongdoing.
* **Why this score** — the principal wallet-wide drivers, including FATF category matches, cluster risk, and confirmed external reports. Each driver has its own impact level; these values do not add up directly to the total score.
* **FATF indicators** — flagged counterparties grouped by category, direction of funds, interaction count, and risk level. Rows are evaluated independently and should not be added together.
* **Wallet exposure** — a composition view showing each category’s share of the screened historical exposure. These percentages are exposure shares, **not** risk scores and **not** percentages of the current wallet balance.
* **Calculation ID and PDF export** — the calculation ID identifies the exact screening result. Save it with the PDF and timestamp so the decision can be reconstructed later.

{% hint style="success" %}
Every check can be **exported as a PDF** — address, score, exposure breakdown and flags — ready to attach to a case file. Checks are also stored server-side under your account.
{% endhint %}

#### Telegram bot (coming soon)

Individuals can configure screenings and receive reports inside **@GetBlockAMLBot** — no separate dashboard needed (Coming soon).

{% hint style="warning" %}
Reports are generated for informational purposes only. Risk scores are probabilistic and provided "as is" — they must not be the sole basis for regulatory, legal or financial decisions.
{% endhint %}
