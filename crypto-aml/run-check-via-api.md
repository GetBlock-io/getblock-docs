---
hidden: true
---

# Run check via API

## Using the AML API

GetBlock Crypto AML screens crypto wallets and transactions for money-laundering and sanctions risk. Send an address or a transaction hash, get back a JSON risk report — a 0–100 score, the components that produced it, and attribution.

Two endpoints, one for each target:

| Target                                                        | Endpoint                    |
| ------------------------------------------------------------- | --------------------------- |
| [A wallet](run-check-via-api.md#screening-a-wallet)           | `POST /v1/aml/wallet-check` |
| [A transaction](run-check-via-api.md#screening-a-transaction) | `POST /v1/aml/tx-check`     |

Both are bearer-authenticated, billed per check from your prepaid balance, and return the report inside a `data` envelope.

### Authentication

The AML API uses bearer auth with a single token per account — the same token that powers UI billing.

```http
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json
```

Generate a token in the dashboard: **API keys** → [account.getblock.io/settings/api-keys](https://account.getblock.io/settings/api-keys)

**Base URL:** `https://services.getblock.io`

### Supported networks

| Value | Network             |
| ----- | ------------------- |
| `eth` | Ethereum            |
| `trx` | TRON                |
| `btc` | Bitcoin             |
| `ltc` | Litecoin            |
| `bch` | Bitcoin Cash        |
| —     | TON _(coming soon)_ |

Values are case-insensitive. Any other value returns `400`:

```json
{ "data": null, "error": "Supported networks: bch, btc, eth, ltc, trx" }
```

***

## Screening a wallet

Covers every asset the address holds, in one report. Use it to check a counterparty before you transact, or to screen a wallet at onboarding.

#### Request

`POST /v1/aml/wallet-check`

| Field     | Type   | Required | Description                                                              |
| --------- | ------ | -------- | ------------------------------------------------------------------------ |
| `address` | string | yes      | Wallet address to screen. Must be valid for `network`.                   |
| `network` | string | yes      | See [supported networks](run-check-via-api.md#supported-networks) above. |

```bash
curl -X POST "https://services.getblock.io/v1/aml/wallet-check" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"address":"0x8589427373D6D84E98730D7795D8f6f8731FDA16","network":"eth"}'
```

#### Response

```json
{
  "data": {
    "contractVersion": "2",
    "coverage": "walletOverall",
    "overallRiskScore": 100,
    "tokenList": [],
    "breakdown": {
      "history": {
        "score": 40.15,
        "tags": [
          {
            "category": "dex",
            "share": 99.95
          },
          {
            "category": "hacking",
            "share": 0.05
          }
        ]
      },
      "externalReports": {
        "score": 100
      },
      "fatf": {
        "score": 100,
        "flags": [
          {
            "flow": "out",
            "reason": "hacking",
            "count": 5,
            "score": 75
          },
          {
            "flow": "in",
            "reason": "phishing",
            "count": 1,
            "score": 75
          }
        ]
      },
      "tokenFlags": {
        "score": 0
      }
    },
    "attribution": {
      "cluster": {
        "names": [
          "Ronin Bridge Exploiter",
          "OFAC SDN LAZARUS GROUP 13-09-2019"
        ],
        "score": 100
      },
      "categories": [
        "blacklist-usdc",
        "blacklist-usdt"
      ]
    },
    "tokenRisks": {},
    "calculationUid": "2329ec43-8275-4e6f-a07c-0f27807fa586"
  }
}

```

| Field                       | Type      | Description                                                                                                                |
| --------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------- |
| `contractVersion`           | string    | Response-contract version.                                                                                                 |
| `coverage`                  | string    | Scope of the result. `walletOverall` means the score applies to the wallet as a whole on the selected network.             |
| `overallRiskScore`          | number    | Wallet-wide risk score from `0` to `100`. This is the headline result.                                                     |
| `tokenList`                 | string\[] | Assets included in token-specific exposure.                                                                                |
| `breakdown`                 | object    | Wallet-wide evidence supporting the result.                                                                                |
| `breakdown.history`         | object    | Historical wallet exposure. Read its risk value from `score`; `tags`, when present, describe the exposure composition.     |
| `breakdown.externalReports` | object    | Risk derived from confirmed external reports.                                                                              |
| `breakdown.fatf`            | object    | FATF-relevant counterparty indicators. Optional `flags` contain direction, reason, interaction count and individual score. |
| `breakdown.tokenFlags`      | object    | Wallet-wide token-related flags. Present structurally, but may contain only `score: 0`.                                    |
| `attribution`               | object    | Identity, cluster and external-list information associated with the address.                                               |
| `attribution.owners`        | string\[] | Known entities believed to control the address. Owner is an identity signal and has no separate score. Optional.           |
| `attribution.cluster`       | object    | On-chain cluster attribution. Its `score` is always present; `names` and `tags` are optional.                              |
| `attribution.categories`    | string\[] | External-list labels such as issuer freezes, sanctions references or functional roles. Optional                            |
| `calculationUid`            | string    | Identifier of this screening calculation. Store it with the result and decision. Optional.                                 |

Each `*_risk` and `*_flags` field is an object — read `cluster_risk.risk_score`, not `cluster_risk`. Only `overall_risk_score` and `risk_score` are guaranteed; treat the rest as optional.

**Read the blocks, not just the score.** In the example above the wallet is nearly empty and every block is `0` except `cluster_risk: 70` — the score comes entirely from the cluster it belongs to, which `owners` names as Tornado Cash.

***

## Screening a transaction

Screens one transferred asset within a transaction, with its senders and receivers.

#### Request

`POST /v1/aml/tx-check`

| Field     | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| `tx`      | string | yes      | Transaction hash to screen.                                    |
| `network` | string | yes      | See supported networks above.                                  |
| `asset`   | string | no       | Which transferred asset to score. Defaults to the native coin. |

A transaction carries specific transfers — the native coin, a token, or both. Each has its own counterparties and is scored on its own, so `asset` selects which transfer to score:

| Network      | Native | Tokens         |
| ------------ | ------ | -------------- |
| Ethereum     | `eth`  | `usdt`, `usdc` |
| TRON         | `trx`  | `usdt`         |
| Bitcoin      | `btc`  | —              |
| Litecoin     | `ltc`  | —              |
| Bitcoin Cash | `bch`  | —              |

Omit `asset` and the native coin is screened. More tokens are in the pipeline.

**Name the asset you're screening** — a check on a USDT payment should carry `"asset": "usdt"`, so the report covers the transfer you actually care about.

```bash
curl -X POST "https://services.getblock.io/v1/aml/tx-check" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"tx":"YOUR_TX_HASH","network":"trx","asset":"usdt"}'
```

#### Response

```json
{
  "data": {
    "risk_score": 100,
    "addresses_in": {
      "0x1a2a1c938ce3ec39b6d47113c7955baa9dd454f2": 40,
      "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2": 34.85
    },
    "addresses_out": {
      "0x098b716b8aaf21512996dc57eb0615e2383e2f96": 100,
      "0x1a2a1c938ce3ec39b6d47113c7955baa9dd454f2": 40
    },
    "risk_tags": [
      { "tag": "dex(defi)",  "percent": 82.58 },
      { "tag": "exchange",    "percent": 9.39 },
      { "tag": "lending",     "percent": 4.35 },
      { "tag": "marketplace", "percent": 2.34 },
      { "tag": "hot_wallet",  "percent": 0.29 },
      { "tag": "mixer",       "percent": 0.15 },
      { "tag": "sanction",    "percent": 0.01 }
    ]
  }
}

```

.

***

### Reading the score

The headline number is 0–100, higher is riskier — `overall_risk_score` for wallets, `risk_score` for transactions.

Thresholds are your policy, not a verdict from the API. A common starting point: 0–24 proceed · 25–49 light review · 50–74 manual review · 75–100 block pending enhanced due diligence.

Scores move over time as a wallet keeps transacting. Store the score and the `calculation_uid` you acted on, so a decision can be reconstructed later.



{% hint style="info" %}
**How to use these scores**

Every check runs a deep analysis. We trace the wallet's transaction graph in both directions — inbound and outbound, across multiple hops — resolve cluster membership to establish who controls the funds, and score the wallet's history and its current holdings as separate questions. On top of that sit our own attribution work, machine learning models, and continuously updated sanctions and blacklist data. That is why a report tells you _where_ risk comes from, not just how much.

**The result is an estimate, and it moves.** Risk scoring is probabilistic by nature. The same address can score differently over time as it keeps transacting, as counterparties change, and as attribution and sanctions data are updated. A score is a considered judgement built from many weighted signals — not a fixed property of the address.

**Use it to decide how much scrutiny a case deserves.** The right question is rarely "is this wallet good or bad?" — it is "does this one need enhanced due diligence, or can it proceed?" Read the risk blocks, not just the headline number: a `100` driven by `cluster_risk` on a regulated exchange means something very different from a `100` driven by `coins_risk` and `fatf_flags`. Set your own thresholds, apply your own policy, and record the `calculation_uid` you acted on so the decision can be reconstructed later.

**KYT is coming.** We're building a Know Your Transaction tool — a visual graph of an address, its counterparties and their interconnections — for cases where you need to trace the origin of funds yourself. Stay tuned.
{% endhint %}

{% hint style="warning" %}
Risk scores are probabilistic and provided "as is." Use them to inform your own risk decisions — not as the sole basis for regulatory, legal or financial decisions.
{% endhint %}



***

**See also:** Using AML via the API · How risk scoring works · Using AML via the UI
