# Run check via API

**GetBlock Crypto AML** also vailable via API. It return a **JSON** with a risk score, exposure breakdown and flags.&#x20;

### Authentication

The AML API uses **bearer auth with a single token per account** — the same token that powers UI billing.

```http
Authorization: Bearer YOUR_API_TOKEN
```

Generate a token from the dashboard: **API keys** → [https://account.getblock.io/settings/api-keys](https://account.getblock.io/settings/api-keys)

**Base URL:** [https://services.getblock.io/](https://services.getblock.io/)&#x20;

#### Response schema

The `v2` (detailed) address endpoint returns:

| Field                 | Type      | Description                                                                   |
| --------------------- | --------- | ----------------------------------------------------------------------------- |
| `address`             | string    | The screened address (or `hash` for transactions).                            |
| `network`             | string    | Network tag, e.g. `ETH`.                                                      |
| `riskScore`           | number    | Composite `0–100` score.                                                      |
| `riskBand`            | string    | `low` \| `medium` \| `high` \| `very-high`.                                   |
| `message`             | string    | Human-readable summary for the band.                                          |
| `exposure`            | array     | `{ category, share }` — where risk comes from; `share` is `0–1`.              |
| `exposure[].category` | string    | `sanctioned_entity` \| `darknet_market` \| `mixer` \| `ransomware` \| `scam`. |
| `exposure[].share`    | number    | Fraction of total exposure (`0–1`).                                           |
| `fatfFlags`           | string\[] | e.g. `sanctions_list_match`, `darknet_market_exposure`, `mixer_interaction`.  |
| `attribution`         | object    | `{ cluster, labels[] }` — cluster id and entity/tag labels (detailed view).   |
| `checkedAt`           | string    | ISO 8601 timestamp of the check.                                              |

### Examples

#### Sample response

```json
{
  "address": "0x6e9a...4b1d",
  "network": "ETH",
  "riskScore": 68,
  "riskBand": "high",
  "message": "Notable exposure to risky counterparties. Review details before accepting funds.",
  "exposure": [
    { "category": "sanctioned_entity", "share": 0.34 },
    { "category": "darknet_market",   "share": 0.22 },
    { "category": "mixer",             "share": 0.18 }
  ],
  "fatfFlags": [
    "sanctions_list_match",
    "darknet_market_exposure",
    "mixer_interaction"
  ],
  "attribution": {
    "cluster": "cluster_4f2a",
    "labels": ["sanctioned_entity", "mixer"]
  },
  "checkedAt": "2026-04-24T09:14:22Z"
}
```

{% hint style="warning" %}
Risk scores are probabilistic and provided "as is." Use them to inform your own risk decisions — not as the sole basis for regulatory, legal or financial decisions.
{% endhint %}
