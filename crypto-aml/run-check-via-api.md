# Run check via API

## Using the AML API

GetBlock Crypto AML screens cryptocurrency wallets and transactions for money-laundering, sanctions, fraud, and other compliance risks.

Send a wallet address or transaction hash and receive a JSON report containing a 0–100 risk score, supporting risk components, exposure composition, counterparties, and available attribution.

### Endpoints

| Target      | Endpoint                    | Coverage                                                               |
| ----------- | --------------------------- | ---------------------------------------------------------------------- |
| Wallet      | `POST /v1/aml/wallet-check` | Screens the wallet across all supported assets on the selected network |
| Transaction | `POST /v1/aml/tx-check`     | Screens one selected asset transfer within a transaction               |

Both endpoints:

* use bearer authentication;
* are billed per successful check from the account’s prepaid balance;
* return the report inside a `data` envelope;
* use the public GetBlock AML taxonomy rather than internal data-source labels.

### Authentication

Create an API key in the GetBlock dashboard:

[Dashboard → API Keys](https://account.getblock.io/settings/api-keys)

The API key identifies the account whose prepaid balance will be charged.

Include it in every request:

```http
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json
```

Base URL:

```
https://services.getblock.io
```

### Supported networks

Network values are case-insensitive.

| Network      | Value | Transaction assets    |
| ------------ | ----- | --------------------- |
| Ethereum     | `eth` | `eth`, `usdt`, `usdc` |
| TRON         | `trx` | `trx`, `usdt`         |
| Bitcoin      | `btc` | `btc`                 |
| Litecoin     | `ltc` | `ltc`                 |
| Bitcoin Cash | `bch` | `bch`                 |

An unsupported network returns HTTP `400`:

```json
{
  "data": null,
  "error": "Supported networks: bch, btc, eth, ltc, trx"
}
```

## Screening a wallet

A wallet check covers the wallet’s overall exposure across every asset available for the selected network.

Use it to screen a counterparty before a transaction, during customer onboarding, or as part of an ongoing compliance review.

The wallet endpoint does not accept an `asset` or currency-tag parameter.

### Request

```http
POST /v1/aml/wallet-check
```

```bash
curl -X POST "https://services.getblock.io/v1/aml/wallet-check" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "address": "0x8589427373D6D84E98730D7795D8f6f8731FDA16",
    "network": "eth"
  }'
```

### Response

```json
{
  "data": {
    "schemaVersion": "1.0",
    "overallRiskScore": 100,
    "breakdown": {
      "exposure": {
        "score": 82.4,
        "activityExposure": [
          {
            "category": "hacking",
            "share": 12.3
          },
          {
            "category": "userAccountCompromise",
            "share": 22.5
          },
          {
            "category": "stolenAssets",
            "share": 8.2
          }
        ],
        "complianceExposure": [
          {
            "category": "sanctionsExposure",
            "share": 11.5,
            "authorities": [
              {
                "authority": "UK",
                "share": 2.5
              },
              {
                "authority": "EU",
                "share": 3.5
              },
              {
                "authority": "UN",
                "share": 1.5
              }
            ],
            "authorityNotProvidedShare": 4
          },
          {
            "category": "jurisdictionalRiskContext",
            "share": 8,
            "details": [
              {
                "category": "sanctionsRestrictedJurisdiction",
                "share": 3
              },
              {
                "category": "fatfCallForActionJurisdiction",
                "share": 5
              }
            ]
          }
        ],
        "counterpartyExposure": [
          {
            "category": "decentralizedExchange",
            "share": 19.2
          },
          {
            "category": "centralizedExchange",
            "share": 7.2
          }
        ],

      },
      "externalReports": {
        "score": 75,
        "activityExposure": [
          {
            "category": "fraudulentSchemes",
            "share": 100
          }
        ]
      },
      "fatf": {
        "score": 100,
        "flags": [
          {
            "flow": "in",
            "category": "sanctionsExposure",
            "count": 5,
            "authorities": [
              "UN"
            ]
          },
          {
            "flow": "out",
            "category": "hacking",
            "count": 17
          }
        ]
      },
      "tokenFlags": {
        "score": 100,
        "flags": [
          {
            "category": "assetRestriction",
            "count": 1,
            "asset": "usdt"
          }
        ]
      }
    },
    "attribution": {
      "owners": [
        "Known service operator"
      ],
      "cluster": {
        "names": [
          "Exploit-related cluster"
        ],
        "score": 100,
        "activityExposure": [
          {
            "category": "hacking",
            "share": 100
          }
        ]
      },
      "assetRestrictions": [
        {
          "asset": "usdt"
        }
      ]
    },
    "reportId": "fa78dfdc-e69d-4bf6-a092-f824bbb24721"
  }
}
```

### Reading a wallet report

#### `schemaVersion`

The version of the customer-facing GetBlock AML response schema.

Use this field when maintaining integrations that may need to support more than one schema version.

#### `overallRiskScore`

The wallet’s headline risk score from `0` to `100`.

Higher values indicate greater identified risk. The score should be interpreted together with the supporting breakdown and attribution.

#### `breakdown.exposure`

The wallet’s observed exposure profile.

It contains:

* `score` — the risk score associated with the exposure component;
* `activityExposure` — exposure to risky or illicit activity;
* `complianceExposure` — sanctions, jurisdictional, regulatory, or compliance-related exposure;
* `counterpartyExposure` — exposure associated with identified entity or service types;
* `unclassifiedExposure` — exposure that could not be assigned to a named public category.

{% hint style="info" %}
A category’s `share` is its share of the exposure composition. It is not a risk score and is not the percentage by which that category increased `overallRiskScore`.
{% endhint %}

#### `breakdown.externalReports`

Risk supported by external reports or identified reports associated with the wallet.

The component has its own `score` and may include categorized exposure.

#### `breakdown.fatf`

Direct compliance-relevant flags associated with wallet interactions.

Each flag may contain:

* `flow` — `in` or `out`;
* `category` — the public risk category;
* `count` — the reported amount or occurrence count;
* `authorities` — identified sanctions authorities, when available;
* `details` — reviewed details for grouped compliance categories, when available.

#### `breakdown.tokenFlags`

Reviewed restrictions associated with particular assets.

This is not a per-token risk breakdown. It reports specific token-related flags such as an issuer restriction.

#### `attribution.owners`

Known entities to which the wallet has been attributed.

This field is omitted when no reviewed owner attribution is available.

#### `attribution.cluster`

Information about the wallet cluster:

* `names` — known cluster names;
* `score` — the risk score assigned to the cluster;
* exposure arrays — categorized evidence associated with the cluster.

A wallet can have limited activity of its own while still carrying material risk because it belongs to an identified cluster.

#### `attribution.assetRestrictions`

Reviewed asset restrictions associated with the wallet.

Only supported and reviewed asset symbols are returned.

#### `reportId`

The unique identifier of the screening report.

Store it alongside the customer, transaction, or compliance decision for audit and support purposes.

### Optional fields

The following fields are returned only when relevant data is available:

* exposure category arrays;
* `unclassifiedExposure`;
* `flags`;
* `owners`;
* cluster `names`;
* `assetRestrictions`;
* `authorities`;
* `details`;
* `reportId`.

Do not assume that every optional array will be present. A missing field means that the report has no public data for that field; it does not change the headline score.

## Screening a transaction

A transaction check evaluates one selected asset transfer within a transaction.

A single blockchain transaction can contain both native-asset and token transfers. These transfers may involve different counterparties and produce different risk results.

For example, an Ethereum transaction may contain both ETH and USDT transfers. Screening `eth` does not automatically screen its `usdt` transfer.

### Request

```http
POST /v1/aml/tx-check
```

```bash
curl -X POST "https://services.getblock.io/v1/aml/tx-check" \
  -H "Authorization: Bearer YOUR_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "tx": "0x5c504ed432cb51138bcf09aa5e8a410dd4a1e204ef84bfed1be16dfba1b22060",
    "network": "eth",
    "asset": "usdt"
  }'
```

Request fields:

| Field     | Required | Description                                                                |
| --------- | -------- | -------------------------------------------------------------------------- |
| `tx`      | Yes      | Transaction hash: 64 hexadecimal characters, optionally prefixed with `0x` |
| `network` | Yes      | Blockchain network                                                         |
| `asset`   | No       | Asset transfer to screen; defaults to the network’s native asset           |

{% hint style="warning" %}
Explicitly provide `asset` for Ethereum and TRON transactions. If it is omitted, the API screens only the network’s native asset.
{% endhint %}

### Response

```json
{
  "data": {
    "schemaVersion": "1.0",
    "riskScore": 100,
    "counterparties": {
      "in": {
        "0xac4cc4b68ea24bbfaac8fd127b67ed445accce22": 100
      },
      "out": {
        "0xa9d1e08c7793af67e9d92fe308d5697fb81d3e43": 10
      }
    },
    "activityExposure": [
      {
        "category": "hacking",
        "share": 20
      },
      {
        "category": "userAccountCompromise",
        "share": 15
      }
    ],
    "complianceExposure": [
      {
        "category": "sanctionsExposure",
        "share": 45,
        "authorities": [
          {
            "authority": "UK",
            "share": 15
          },
          {
            "authority": "EU",
            "share": 10
          },
          {
            "authority": "UN",
            "share": 10
          }
        ],
        "authorityNotProvidedShare": 10
      }
    ],
    "counterpartyExposure": [
      {
        "category": "decentralizedExchange",
        "share": 10
      }
    ],
  
}
```

### Reading a transaction report

#### `riskScore`

The transaction’s headline risk score from `0` to `100`.

#### `counterparties`

Addresses involved in the screened asset transfer, grouped by flow:

* `in` — incoming value flow;
* `out` — outgoing value flow.

Each object maps a counterparty address to its risk score.

#### Exposure arrays

Transaction exposure is separated into:

* `activityExposure`;
* `complianceExposure`;
* `counterpartyExposure`.

Each entry contains a public `category` and its `share` of the transaction’s exposure composition.

#### `unclassifiedExposure`

The share of exposure not assigned to a named public category.

The share remains visible so that the exposure composition stays complete, but internal or unreviewed classification names are not returned.

{% hint style="info" %}
The transaction response does not currently repeat the submitted `tx`, `network`, or `asset`. Store the request context together with the response if it is required for audit records.
{% endhint %}

## Important interpretation rules

1. Use `overallRiskScore` for wallet reports and `riskScore` for transaction reports.
2. Read the supporting components and attribution before making a decision.
3. Do not treat an exposure `share` as a risk score or model weight.
4. Do not calculate the headline score by averaging category shares.
5. A category may represent grouped evidence from several internal sources.
6. `unclassifiedExposure` means that exposure exists but is not assigned to a named public category.
7. Missing optional fields do not mean that the entire check failed.
8. Store `reportId` when it is present.

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

Scores move over time as a wallet keeps transacting. Store the score and the `calculation_uid` you acted on, so a decision can be reconstructed later.

{% hint style="info" %}
**How to use these scores**

Every check runs a deep analysis. We trace the wallet's transaction graph in both directions — inbound and outbound, across multiple hops — resolve cluster membership to establish who controls the funds, and score the wallet's history and its current holdings as separate questions. On top of that sit our own attribution work, machine learning models, and continuously updated sanctions and blacklist data. That is why a report tells you _where_ risk comes from, not just how much.

**The result is an estimate, and it moves.** Risk scoring is probabilistic by nature. The same address can score differently over time as it keeps transacting, as counterparties change, and as attribution and sanctions data are updated. A score is a considered judgement built from many weighted signals — not a fixed property of the address.

**Use it to decide how much scrutiny a case deserves.** The right question is rarely "is this wallet good or bad?" — it is "does this one need enhanced due diligence, or can it proceed?".&#x20;

**KYT is coming.** We're building a Know Your Transaction tool — a visual graph of an address, its counterparties and their interconnections — for cases where you need to trace the origin of funds yourself. Stay tuned.
{% endhint %}

{% hint style="warning" %}
Risk scores are probabilistic and provided "as is." Use them to inform your own risk decisions — not as the sole basis for regulatory, legal or financial decisions.
{% endhint %}

***

**See also:** Using AML via the API · How risk scoring works · Using AML via the UI
