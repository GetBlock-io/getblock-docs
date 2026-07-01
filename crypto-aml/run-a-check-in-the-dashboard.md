# Run a check in the dashboard

The web UI is the fastest way to screen a wallet or transaction — no code required.&#x20;

{% hint style="info" %}
Getting started takes three things: sign up with an email, top up a small balance, and start screening. **No KYB and no annual contract.**
{% endhint %}

Entry point: [https://account.getblock.io/products/crypto-aml](https://account.getblock.io/products)→ **Run a check**.

### Screening in three steps

The individual flow is a simple, three-step screen:

<table><thead><tr><th width="86.8984375">Step</th><th>Action</th></tr></thead><tbody><tr><td><strong>01</strong></td><td><strong>Pick a network</strong> — <code>ETH</code>, <code>TRX</code> , <code>BTC</code>,  <code>LTC</code>, <code>BCH</code> </td></tr><tr><td><strong>02</strong></td><td><strong>Paste a wallet or transaction</strong> — an address or a transaction hash.</td></tr><tr><td><strong>03</strong></td><td><strong>Read the report</strong> — a risk score, exposure breakdown, and FATF flags in seconds.</td></tr></tbody></table>

#### Reading the result screen

The report ("**Address risk — Detailed view**") surfaces:

* **Risk score** — the `0–100` composite, with a colour-coded band (Low / Medium / High / Very high) and a short, human-readable **message**. See risk bands.
* **Exposure breakdown** — a composition chart of where the risk comes from (sanctioned entity, darknet, mixer, ransomware, scam, clean counterparties).
* **FATF flags** — discrete regulatory indicators (e.g. _Sanctions list match_, _Mixer / tumbler interaction_).
* **Token-level breakdown** — for multi-token wallets, each asset is scored independently so a clean ETH balance doesn't mask a risky USDT flow.
* **Historical vs current exposure** and **direct interaction risk** (incoming and outgoing flow scored separately).

{% hint style="success" %}
Every check can be **exported as a PDF** — address, score, exposure breakdown and flags — ready to attach to a case file. Checks are also stored server-side under your account.
{% endhint %}

#### Telegram bot

Individuals can configure screenings and receive reports inside **@GetBlockAMLBot** — no separate dashboard needed (Coming soon).

### UI states to expect

| State                   | What you see                                                            |
| ----------------------- | ----------------------------------------------------------------------- |
| **Loading**             | The check runs server-side;                                             |
| **Result**              | The report screen described above.                                      |
| **Empty / new address** | A wallet with little on-chain history returns a low-signal result.      |
| **Error**               | Invalid address/hash for the selected network, or insufficient balance. |

{% hint style="warning" %}
Reports are generated for informational purposes only. Risk scores are probabilistic and provided "as is" — they must not be the sole basis for regulatory, legal or financial decisions.
{% endhint %}
