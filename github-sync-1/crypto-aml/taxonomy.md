# Taxonomy

The GetBlock AML Risk Taxonomy provides a consistent, customer-facing language for describing the risk context of crypto wallets and transactions.

An AML report does more than return a single score. It explains:

* the headline risk of the wallet or transaction;
* the types of activity found in screened exposure;
* compliance-related context that may require review;
* the types of counterparties or services involved;
* the share associated with each exposure category;
* direct interactions with flagged counterparties;
* ownership and cluster attribution, when available;
* asset-specific restrictions;
* exposure or flags that could not be assigned to a named public category.

The current customer-facing response schema is identified by:

```json
{
  "schemaVersion": "1.0"
}
```

{% hint style="info" %}
A category describes the nature or context of an AML signal. It is not, by itself, a legal conclusion, proof of wrongdoing, or an instruction to reject a customer or transaction.
{% endhint %}

## Activity exposure

Activity exposure describes potentially harmful, illicit, abusive, or high-risk activity associated with a wallet, transaction, cluster, or counterparty.

### Hacking

**API value:** `hacking`\
**Interface presentation:** Very high

Activity associated with unauthorized access to systems, wallets, smart contracts, protocols, or digital infrastructure.

This may indicate exploitation, unauthorized fund movement, or infrastructure compromise.

**Recommended review:** Examine transaction timing, counterparties, known incidents, and whether the assets may originate from an exploit.

### User or device compromise

**API value:** `userAccountCompromise`\
**Interface presentation:** Very high

Activity associated with compromised accounts, private keys, devices, credentials, or user access.

This can include phishing-related compromise, malicious software, credential theft, or unauthorized access to a user-controlled wallet.

**Recommended review:** Confirm wallet ownership and investigate unusual access or transfer patterns.

### Extortion activity

**API value:** `extortionActivity`\
**Interface presentation:** Very high

Activity associated with coercive payment demands, threats, ransom demands, blackmail, or other forms of financial extortion.

**Recommended review:** Escalate for enhanced due diligence and preserve relevant screening and transaction evidence.

### Fraudulent schemes

**API value:** `fraudulentSchemes`\
**Interface presentation:** Very high

Activity associated with deceptive investment programs, fraudulent services, scams, impersonation, Ponzi-like schemes, or other attempts to obtain funds through misrepresentation.

**Recommended review:** Verify the counterparty’s identity, business model, transaction purpose, and available fraud reports.

### Stolen assets

**API value:** `stolenAssets`\
**Interface presentation:** Very high

Assets or activity associated with theft, unauthorized transfer, misappropriation, or movement of funds identified as stolen.

**Recommended review:** Review transaction path, timing, value, proximity, and available incident information.

### Darknet activity

**API value:** `darknetActivity`\
**Interface presentation:** Very high

Activity associated with darknet markets, hidden services, or infrastructure used to facilitate illicit commerce.

**Recommended review:** Assess the directness of exposure, transaction history, amounts, and applicable prohibited-use policies.

### Drug trafficking

**API value:** `drugTrafficking`\
**Interface presentation:** Very high

Activity associated with the illegal production, sale, distribution, financing, or movement of proceeds related to controlled drugs or narcotic substances.

**Recommended review:** Escalate under the organization’s financial-crime procedures.

### Weapons trafficking

**API value:** `weaponsTrafficking`\
**Interface presentation:** Very high

Activity associated with the illegal sale, distribution, financing, transfer, or acquisition of weapons.

**Recommended review:** Treat as a high-priority financial-crime signal.

### Child abuse

**API value:** `childAbuse`\
**Interface presentation:** Very high

Activity associated with the financing, distribution, or commercial infrastructure of child abuse or exploitation.

**Recommended review:** Treat as a critical signal and follow applicable escalation and reporting requirements.

### Human exploitation

**API value:** `humanExploitation`\
**Interface presentation:** Very high

Activity associated with human trafficking, forced labour, sexual exploitation, or other forms of organized human abuse.

**Recommended review:** Escalate for immediate enhanced review.

### Terrorism financing

**API value:** `terrorismFinancing`\
**Interface presentation:** Very high

Activity potentially associated with raising, moving, storing, or using funds to support terrorist individuals, organizations, or operations.

**Recommended review:** Treat as a critical compliance signal and verify applicable legal obligations.

### Extremist activity

**API value:** `extremistActivity`\
**Interface presentation:** High

Activity associated with extremist individuals, organizations, fundraising, propaganda infrastructure, or related financial networks.

**Recommended review:** Examine the nature and directness of the association.

### Weapons proliferation financing

**API value:** `weaponsProliferationFinancing`\
**Interface presentation:** Very high

Activity potentially associated with financing the development, acquisition, transfer, or proliferation of prohibited weapons programs or related materials.

**Recommended review:** Treat as a critical signal requiring specialist compliance review.

### Transaction obfuscation

**API value:** `transactionObfuscation`\
**Interface presentation:** High

Activity involving services or techniques intended to make the origin, destination, ownership, or movement of assets more difficult to determine.

This may include mixing or other privacy-enhancing transaction patterns.

{% hint style="info" %}
Privacy-enhancing activity is not automatically illegal. Its significance depends on transaction purpose, jurisdiction, counterparties, and the customer’s risk profile.
{% endhint %}

**Recommended review:** Examine transaction paths, timing, counterparties, and the business reason for the activity.

### Gambling

**API value:** `gambling`\
**Interface presentation:** High

Exposure to betting, wagering, gaming, or gambling-related services.

The regulatory significance depends on licensing, jurisdiction, transaction purpose, and the customer’s profile.

### Illegal activity

**API value:** `illegalActivity`\
**Interface presentation:** Very high

Activity classified as illegal that is not represented more precisely by another named activity category.

**Recommended review:** Treat as a material signal and examine the complete report and transaction history.

## Compliance exposure

Compliance exposure describes sanctions, jurisdictional, regulatory, legal, and due-diligence context.

### Sanctions exposure

**API value:** `sanctionsExposure`\
**Interface presentation:** Very high

Potential exposure to a person, entity, organization, address, or activity associated with sanctions-related measures.

When publicly identified, the response can divide the exposure by authority:

| Authority      | API value |
| -------------- | --------- |
| United Kingdom | `UK`      |
| European Union | `EU`      |
| United Nations | `UN`      |

Example:

```json
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
}
```

{% hint style="warning" %}
Sanctions exposure is a screening signal, not a final legal determination. Confirm possible matches against the lists and legal requirements applicable to your organization.
{% endhint %}

**Recommended review:**

1. Verify the relevant person or entity.
2. Check the applicable sanctions lists.
3. Review whether the exposure is direct or indirect.
4. Consider transaction timing and ownership information.
5. Escalate according to your sanctions policy.

### Jurisdictional risk context

**API value:** `jurisdictionalRiskContext`

Exposure associated with a jurisdiction requiring additional sanctions, AML, or counter-terrorist-financing scrutiny.

This category can contain the following public details:

| Detail                            | API value                         | Interface presentation | Meaning                                                                            |
| --------------------------------- | --------------------------------- | ---------------------- | ---------------------------------------------------------------------------------- |
| Sanctions-restricted jurisdiction | `sanctionsRestrictedJurisdiction` | Very high              | The jurisdiction is associated with sanctions-related restrictions.                |
| FATF call-for-action jurisdiction | `fatfCallForActionJurisdiction`   | High                   | The jurisdiction is identified as high risk and subject to a FATF call for action. |

Example:

```json
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
```

The detail shares use the same exposure basis as the parent category and normally reconcile to its share.

{% hint style="info" %}
Jurisdictional risk context does not mean that the person, wallet, or transaction is individually sanctioned. A jurisdictional association and an individual sanctions designation are different signals.
{% endhint %}

### Limited compliance controls

**API value:** `limitedComplianceControls`\
**Interface presentation:** High

Exposure to a service or counterparty whose compliance controls appear limited, insufficiently transparent, or below the level expected for the relevant activity.

This can indicate additional due-diligence risk involving customer verification, transaction monitoring, sanctions screening, licensing, or operational accountability.

**Recommended review:** Verify the service, licensing status, jurisdiction, ownership, and compliance practices.

### Regulatory attention

**API value:** `regulatoryAttention`

Activity associated with regulatory, legal, judicial, enforcement, or public-office-related attention.

{% hint style="info" %}
Regulatory attention does not necessarily establish wrongdoing. It identifies context that may require additional verification or enhanced due diligence.
{% endhint %}

**Recommended review:** Determine the nature, status, relevance, directness, and recency of the regulatory or legal event.

## Counterparty exposure

Counterparty exposure describes the type of service, organization, protocol, or institution associated with screened exposure.

{% hint style="info" %}
A counterparty category is descriptive. It does not automatically mean that the entity is safe, risky, regulated, unregulated, approved, or prohibited.
{% endhint %}

### Centralized exchange

**API value:** `centralizedExchange`\
**Interface presentation:** Low/informational

A custodial trading platform or exchange that facilitates the purchase, sale, conversion, or transfer of digital assets.

This category identifies the counterparty type. It is not an endorsement of the exchange.

### Off-exchange trading

**API value:** `offExchangeTrading`\
**Interface presentation:** Low/informational

Trading activity outside a centralized exchange order book, including over-the-counter and peer-to-peer trading models.

**Recommended review:** Consider counterparty identity, settlement arrangements, transaction purpose, and relationship transparency.

### Decentralized exchange

**API value:** `decentralizedExchange`\
**Interface presentation:** Medium

An on-chain or non-custodial service that enables digital-asset exchange through smart contracts or liquidity mechanisms.

The category describes a service type. It does not mean that every transaction passing through the service is illicit.

### Lending service

**API value:** `lendingService`\
**Interface presentation:** Low/informational

A service or protocol providing digital-asset lending, borrowing, credit, collateralization, or related financing.

### Staking service

**API value:** `stakingService`\
**Interface presentation:** Low/informational

A service or protocol used to stake digital assets, delegate network participation, or earn staking-related rewards.

### Wallet service

**API value:** `walletService`\
**Interface presentation:** Low/informational

A service providing wallet infrastructure, wallet management, transaction execution, or address management.

This can include hosted, operational, hot-wallet, or cold-wallet infrastructure.

### Custody service

**API value:** `custodyService`\
**Interface presentation:** Low/informational

A service that safeguards, administers, or controls digital assets on behalf of customers or organizations.

### Digital asset payments

**API value:** `digitalAssetPayments`\
**Interface presentation:** Low/informational

A service facilitating crypto payments, merchant settlement, payment processing, remittances, or value conversion.

### Digital marketplace

**API value:** `digitalMarketplace`\
**Interface presentation:** Low/informational

A marketplace where users buy, sell, or exchange goods, services, or digital items using digital assets.

### Investment funding

**API value:** `investmentFunding`\
**Interface presentation:** Medium

Activity associated with investment vehicles, funds, crowdfunding, capital formation, or other fundraising structures.

{% hint style="info" %}
Investment-related activity is not inherently illicit. Review its structure, counterparties, jurisdiction, and purpose.
{% endhint %}

### Charitable activity

**API value:** `charitableActivity`\
**Interface presentation:** Medium

Activity associated with donations, nonprofit organizations, charitable fundraising, or humanitarian activity.

The category should be assessed together with beneficiary, jurisdiction, sanctions, and transaction-purpose information.

### Mining activity

**API value:** `miningActivity`\
**Interface presentation:** Low/informational

Activity associated with cryptocurrency mining, mining pools, hosted mining, or cloud-mining services.

### Blockchain entertainment

**API value:** `blockchainEntertainment`\
**Interface presentation:** Medium

Activity associated with blockchain-based games, entertainment platforms, digital collectibles, or related ecosystems.

### Token faucet

**API value:** `tokenFaucet`\
**Interface presentation:** Low/informational

A service that distributes small amounts of tokens, commonly for testing, onboarding, participation, or promotional purposes.

### Smart contract

**API value:** `smartContract`\
**Interface presentation:** Low/informational

An address identified as executable on-chain code rather than a conventional user-controlled wallet.

A smart-contract classification is contextual and does not itself indicate risk.

### Technology infrastructure

**API value:** `technologyInfrastructure`\
**Interface presentation:** Medium

Infrastructure supporting blockchain or digital-asset services, such as hosting, nodes, routing, development tools, or operational technology.

### Governance entity

**API value:** `governanceEntity`\
**Interface presentation:** Low/informational

A government, political organization, public authority, or entity involved in governance or public administration.

This describes institutional context and does not itself indicate misconduct.

### Civic institution

**API value:** `civicInstitution`\
**Interface presentation:** Low/informational

An educational, media, or other public-interest institution.

### Adult content service

**API value:** `adultContentService`\
**Interface presentation:** High

A service providing adult-oriented content or related commercial activity.

The compliance significance depends on legality, jurisdiction, age controls, payment restrictions, and organizational policy.

## Wallet response structure

A wallet report contains:

```json
{
  "schemaVersion": "1.0",
  "overallRiskScore": 100,
  "breakdown": {
    "exposure": {
      "score": 82.4
    },
    "externalReports": {
      "score": 75
    },
    "fatf": {
      "score": 100
    },
    "tokenFlags": {
      "score": 100
    }
  },
  "attribution": {
    "cluster": {
      "score": 100
    }
  },
  "reportId": "fa78dfdc-e69d-4bf6-a092-f824bbb24721"
}
```

### Wallet fields

| Field                           | Meaning                                                                  |
| ------------------------------- | ------------------------------------------------------------------------ |
| `schemaVersion`                 | Version of the customer-facing JSON structure.                           |
| `overallRiskScore`              | Headline wallet risk score.                                              |
| `breakdown.exposure`            | Screened wallet exposure composition and its component score.            |
| `breakdown.externalReports`     | Confirmed external-report signals and their component score.             |
| `breakdown.fatf`                | Reviewed flagged-counterparty classifications and their component score. |
| `breakdown.tokenFlags`          | Asset-specific flagged interactions and their component score.           |
| `attribution.owners`            | Known owner names, when available.                                       |
| `attribution.cluster`           | Cluster names, score, and optional exposure composition.                 |
| `attribution.assetRestrictions` | Reviewed asset restrictions.                                             |
| `reportId`                      | Identifier for the screening report, when available.                     |

{% hint style="info" %}
The `fatf` component is an analytical response component. Its public categories are GetBlock categories. A FATF-specific term is used only where the response explicitly returns `fatfCallForActionJurisdiction`.
{% endhint %}

## Representative wallet response

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
          },
          {
            "category": "otherComplianceRisk",
            "share": 1.3
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
            "authorities": ["UN"]
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
      "owners": ["Known service operator"],
      "cluster": {
        "names": ["Exploit-related cluster"],
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
