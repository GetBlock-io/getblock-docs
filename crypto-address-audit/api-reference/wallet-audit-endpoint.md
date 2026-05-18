---
description: >-
  Example code for the /wallet-audit/audit method. Сomplete guide on how to use
  /wallet-audit/audit in GetBlock Address Audit documentation.
---

# Wallet Audit Endpoint

This method performs a comprehensive fraud audit on a wallet address. Proxied to ChainAware.

## Body Parameters

| Parameter | Type    | Required | Description                                         |
| --------- | ------- | -------- | --------------------------------------------------- |
| network   |  string | Yes      | The blockchain network to verify(ETH, BNB, or BASE) |
| address   | string  | Yes      | the wallet address                                  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://services.getblock.io/v1/wallet-audit/audit' \
-- header 'Authorization: Bearer YOUR_API_KEY',
--header 'Content-Type: application/json' \
--data-raw ' {  "network": "ETH", "address": "0xa45f...a675"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from "axios";

const data = 
  {  "network": "ETH", "address": "0xa45f...a675"};

const config = {
  method: "post",
  url: "https://services.getblock.io/v1/wallet-audit/audit",
  headers: {
    Authorization: "Bearer YOUR_API_KEY",
    "Content-Type": "application/json",
  },
  data: data,
};

axios(config)
  .then((response) => console.log(JSON.stringify(response.data, null, 2)))
  .catch((error) => console.log(error));

```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://services.getblock.io/v1/wallet-audit/audit"

payload = json.dumps({  "network": "ETH", "address": "0xa45f...a675"})

headers = {
      "Authorization": "Bearer YOUR_API_KEY",
  "Content-Type": "application/json",
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({  "network": "ETH", "address": "0xa45f...a675"});

    let response = client
        .post("https://services.getblock.io/v1/wallet-audit/audit")
        .header("Content-Type", "application/json")
        .header("Authorization: "Bearer YOUR_API_KEY")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "data": {
        "message": "Success",
        "walletAddress": "0xa45f...a675",
        "status": "Fraud",
        "probabilityFraud": "0.9329851866",
        "token": null,
        "chain": "ETH",
        "lastChecked": "2026-03-19T07:02:56.000Z",
        "forensic_details": {
            "cybercrime": "0",
            "money_laundering": "0",
            "number_of_malicious_contracts_created": "0",
            "gas_abuse": "0",
            "financial_crime": "0",
            "darkweb_transactions": "0",
            "reinit": "0",
            "phishing_activities": "0",
            "fake_kyc": "0",
            "blacklist_doubt": "0",
            "fake_standard_interface": "0",
            "data_source": "",
            "stealing_attack": "0",
            "blackmail_activities": "0",
            "sanctioned": "0",
            "malicious_mining_activities": "0",
            "mixer": "0",
            "fake_token": "0",
            "honeypot_related_address": "0"
        },
        "categories": [],
        "riskProfile": [
            {
                "Category": "Stable Coin",
                "Balance_age": 0
            },
            {
                "Category": "1to10",
                "Balance_age": 100
            },
            {
                "Category": "10to20",
                "Balance_age": 0
            },
            {
                "Category": "20to30",
                "Balance_age": 0
            },
            {
                "Category": "30to50",
                "Balance_age": 0
            },
            {
                "Category": "50to100",
                "Balance_age": 0
            },
            {
                "Category": "100to200",
                "Balance_age": 0
            },
            {
                "Category": "200to300",
                "Balance_age": 0
            },
            {
                "Category": "300+",
                "Balance_age": 0
            },
            {
                "Category": "Meme Token",
                "Balance_age": 0
            },
            {
                "Category": "Risk_Profile",
                "Balance_age": 2
            }
        ],
        "segmentInfo": "{\"Maker\":0,\"Aave_borrow\":0,\"Aave_lend\":0,\"Lido\":0,\"Compound_lend\":0,\"Compound_borrow\":0}",
        "experience": {
            "Type": "Experience",
            "Value": 0
        },
        "intention": {
            "Type": "Intentions",
            "Value": {
                "Prob_Borrow": "Low",
                "Prob_Gamble": "Low",
                "Prob_Game": "Medium",
                "Prob_Lend": "High",
                "Prob_Leverage_Long": "Low",
                "Prob_Leverage_Long_ETH": "Low",
                "Prob_Leveraged_Lend": "Low",
                "Prob_Leveraged_Stake": "Low",
                "Prob_Leveraged_Stake_ETH": "Low",
                "Prob_NFT": "Medium",
                "Prob_Stake": "Low",
                "Prob_Stake_ETH": "Medium",
                "Prob_Trade": "High",
                "Prob_Yield_Farm": "Low"
            }
        },
        "protocols": [],
        "userDetails": {
            "wallet_age_days": 349,
            "total_balance_usd": 0.26,
            "transaction_count": 2,
            "wallet_rank": 0
        },
        "riskCapability": 1,
        "recommendation": {
            "Type": "Recommendation",
            "Value": [
                "WBTC holding",
                "ETH holding",
                "Stablecoin lending",
                "Stablecoin lending with FIFs (SmartCredit.io)"
            ]
        },
        "checked_times": 6,
        "createdAt": "2026-03-19T06:52:28.000Z",
        "updatedAt": "2026-05-15T17:17:02.000Z",
        "sanctionData": [
            {
                "id": 205878,
                "trustscore_id": 18360032,
                "category": null,
                "name": null,
                "description": null,
                "url": null,
                "isSanctioned": false,
                "createdAt": "2026-05-15T17:17:02.000Z",
                "updatedAt": "2026-05-15T17:17:02.000Z"
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| **Field Path**                             | **Type**          | **Example**                                            | **Description**                                                                                                                                                                                                                                                 |
| ------------------------------------------ | ----------------- | ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status                                     | string            | `"Not Fraud"`                                          | ML model verdict. Possible values: `"Not Fraud"`, `"Fraud"`, `"New Address"`.                                                                                                                                                                                   |
| probabilityFraud                           | string            | `"0.0191140026"`                                       | Fraud probability score ranging from `0.0` to `1.0`.                                                                                                                                                                                                            |
| walletAddress                              | string            | `"0xa45f..."`                                          | The audited blockchain wallet address.                                                                                                                                                                                                                          |
| chain                                      | string            | `"ETH"`                                                | The network/blockchain name (e.g., `"ETH"`).                                                                                                                                                                                                                    |
| lastChecked                                | string (ISO 8601) | `"2026-04-15T12:02:21Z"`                               | Timestamp of the most recent fraud analysis.                                                                                                                                                                                                                    |
| checked\_times                             | number            | `183`                                                  | Total number of times this address has been checked.                                                                                                                                                                                                            |
| createdAt                                  | string (ISO 8601) | `"2023-07-19T10:15:00Z"`                               | Record creation timestamp.                                                                                                                                                                                                                                      |
| updatedAt                                  | string (ISO 8601) | `"2026-05-08T14:30:22Z"`                               | Last update timestamp.                                                                                                                                                                                                                                          |
| forensic\_details                          | object            | _See sub-fields below_                                 | Contains 18 AML flags. Each is `"0"` (clean) or `"1"` (flagged). If any flag is `"1"`, `probabilityFraud` defaults to `1.0`.                                                                                                                                    |
| ↳ `.cybercrime`                            | string            | `"0"`                                                  | Cybercrime activity.                                                                                                                                                                                                                                            |
| ↳ `.money_laundering`                      | string            | `"0"`                                                  | Money laundering activity.                                                                                                                                                                                                                                      |
| ↳ `.number_of_malicious_contracts_created` | string            | `"0"`                                                  | Creation of malicious smart contracts.                                                                                                                                                                                                                          |
| ↳ `.gas_abuse`                             | string            | `"0"`                                                  | Gas price manipulation behavior.                                                                                                                                                                                                                                |
| ↳ `.financial_crime`                       | string            | `"0"`                                                  | General financial crime flag.                                                                                                                                                                                                                                   |
| ↳ `.darkweb_transactions`                  | string            | `"0"`                                                  | Transactions associated with darkweb marketplaces.                                                                                                                                                                                                              |
| ↳ `.reinit`                                | string            | `"0"`                                                  | Smart contract reinitialization abuse.                                                                                                                                                                                                                          |
| ↳ `.phishing_activities`                   | string            | `"0"`                                                  | Phishing vector participation.                                                                                                                                                                                                                                  |
| ↳ `.fake_kyc`                              | string            | `"0"`                                                  | Use or association with fake KYC data.                                                                                                                                                                                                                          |
| ↳ `.blacklist_doubt`                       | string            | `"0"`                                                  | Association with blacklisted entities.                                                                                                                                                                                                                          |
| ↳ `.fake_standard_interface`               | string            | `"0"`                                                  | Spoofed/fake token interface usage.                                                                                                                                                                                                                             |
| ↳ `.stealing_attack`                       | string            | `"0"`                                                  | Active theft or exploit activity.                                                                                                                                                                                                                               |
| ↳ `.blackmail_activities`                  | string            | `"0"`                                                  | Blackmail or extortion activities.                                                                                                                                                                                                                              |
| ↳ `.sanctioned`                            | string            | `"0"`                                                  | Presence on known sanctions lists.                                                                                                                                                                                                                              |
| ↳ `.malicious_mining_activities`           | string            | `"0"`                                                  | Malicious or hijacked mining operations.                                                                                                                                                                                                                        |
| ↳ `.mixer`                                 | string            | `"0"`                                                  | Interaction with crypto mixing services (e.g., Tornado Cash).                                                                                                                                                                                                   |
| ↳ `.fake_token`                            | string            | `"0"`                                                  | Creation or deployment of fake/honeypot tokens.                                                                                                                                                                                                                 |
| ↳ `.honeypot_related_address`              | string            | `"0"`                                                  | Association with honeypot contract setups.                                                                                                                                                                                                                      |
| categories                                 | array (object)    | `[{"Category": "NFT", "Count": 12}]`                   | Transaction count breakdown by category. Examples: `"Decentralized Exchanges"`, `"Bridge"`, `"NFT"`, `"Layer 1"`, `"Layer 2"`, `"Lending & Borrowing"`.                                                                                                         |
| riskProfile                                | array (object)    | `[{"Category": "Stable Coin", "Balance_age": "..."}]`  | Balance distribution. Categories include: `"Stable Coin"`, `"1to10"`, `"10to20"`, ..., `"300+"`, `"Meme Token"`, `"Risk_Profile"`. _Note: The "Risk\_Profile" entry represents the Risk Willingness score in the UI._                                           |
| segmentInfo                                | string (JSON)     | `"{\"Maker\":0,\"Aave_borrow\":0,...}"`                | Protocol interaction flags packed as a JSON string. `1` = interacted, `0` = not. Supported keys: `Maker`, `Aave_borrow`, `Aave_lend`, `Lido`, `Uniswap`, `Compound_lend`, `Compound_borrow`.                                                                    |
| experience                                 | object            | `{"Type": "Experience", "Value": 8}`                   | User experience rating mapped on a scale of `1-10`.                                                                                                                                                                                                             |
| intention                                  | object            | _See description_                                      | Expected behavior map. Format: `{"Type": "Intentions", "Value": { ... }}` where `Value` contains 14 sub-keys, each rated as `"Low"`, `"Medium"`, or `"High"`.                                                                                                   |
| ↳ _Sub-keys_                               | string            | `"Low"`, `"Medium"`, `"High"`                          | `Prob_Borrow`, `Prob_Gamble`, `Prob_Game`, `Prob_Lend`, `Prob_Leverage_Long`, `Prob_Leverage_Long_ETH`, `Prob_Leveraged_Lend`, `Prob_Leveraged_Stake`, `Prob_Leveraged_Stake_ETH`, `Prob_NFT`, `Prob_Stake`, `Prob_Stake_ETH`, `Prob_Trade`, `Prob_Yield_Farm`. |
| protocols                                  | array (object)    | `[{"Protocol": "Uniswap", "Count": 45}]`               | Array of specific protocols the wallet has interacted with alongside their usage frequency.                                                                                                                                                                     |
| userDetails                                | object            | _See sub-fields below_                                 | General metadata and structural metrics of the wallet.                                                                                                                                                                                                          |
| ↳ `.wallet_age_days`                       | number            | `1024`                                                 | Age of the wallet in days.                                                                                                                                                                                                                                      |
| ↳ `.total_balance_usd`                     | number            | `15430.50`                                             | Current aggregate wallet balance in USD.                                                                                                                                                                                                                        |
| ↳ `.transaction_count`                     | number            | `450`                                                  | Cumulative historical transaction count.                                                                                                                                                                                                                        |
| ↳ `.wallet_rank`                           | number            | `88`                                                   | Internal wallet ranking score.                                                                                                                                                                                                                                  |
| riskCapability                             | number            | `7`                                                    | User's risk capability evaluated on a scale of `1-10`.                                                                                                                                                                                                          |
| recommendation                             | object            | `{"Type": "Recommendation", "Value": ["ETH holding"]}` | Tailored system recommendations. `Value` is an array of strings (e.g., `["ETH holding", "Stablecoin lending"]`).                                                                                                                                                |
| sanctionData                               | array (object)    | _See sub-fields below_                                 | Definitive sanction matches associated with this address.                                                                                                                                                                                                       |
| ↳ `.isSanctioned`                          | boolean           | `false`                                                | `true` if sanctioned, `false` if clean.                                                                                                                                                                                                                         |
| ↳ `.category`                              | string (nullable) | `"Terrorism"`                                          | Category of sanction if applicable; otherwise `null`.                                                                                                                                                                                                           |
| ↳ `.name`                                  | string (nullable) | `"Entity Name"`                                        | Official name of the sanctioned entity/person; otherwise `null`.                                                                                                                                                                                                |
| ↳ `.description`                           | string (nullable) | `"Details here..."`                                    | Contextual details regarding the sanction; otherwise `null`.                                                                                                                                                                                                    |
| ↳ `.url`                                   | string (nullable) | `"https://..."`                                        | Reference URL providing sanction evidence; otherwise `null`.                                                                                                                                                                                                    |

## Error Handling

| Status | Description                       |
| ------ | --------------------------------- |
| 400    | Invalid network or missing fields |
| 401    | Missing or invalid auth           |
| 402    | Insufficient balance              |
| 502    | Provider error                    |
