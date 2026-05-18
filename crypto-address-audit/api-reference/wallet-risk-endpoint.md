---
description: >-
  Example code for the /wallet-audit/check method. Сomplete guide on how to use
  /wallet-audit/check in GetBlock Address Audit documentation.
---

# Wallet Risk Endpoint

This method performs a quick risk score check on a wallet address. Supports more networks than the full audit.

## Body Parameters

| Parameter | Type    | Required | Description                                                   |
| --------- | ------- | -------- | ------------------------------------------------------------- |
| network   |  string | Yes      | The blockchain network to verify(ETH, BNB,POLY, TRON or BASE) |
| address   | string  | Yes      | the wallet address                                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://services.getblock.io/v1/wallet-audit/check' \
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
  url: "https://services.getblock.io/v1/wallet-audit/check",
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

url = "https://services.getblock.io/v1/wallet-audit/check"

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
        .post("https://services.getblock.io/v1/wallet-audit/check")
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
        "checked_times": 7,
        "createdAt": "2026-03-19T06:52:28.000Z",
        "updatedAt": "2026-05-18T11:11:19.000Z",
        "sanctionData": [
            {
                "id": 212143,
                "trustscore_id": 18360032,
                "category": null,
                "name": null,
                "description": null,
                "url": null,
                "isSanctioned": false,
                "createdAt": "2026-05-18T11:11:19.000Z",
                "updatedAt": "2026-05-18T11:11:19.000Z"
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| **Field Path**                             | **Type**          | **Example**              | **Description**                                                                                                              |
| ------------------------------------------ | ----------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| status                                     | string            | `"Not Fraud"`            | ML model verdict. Possible values: `"Not Fraud"`, `"Fraud"`, `"New Address"`.                                                |
| probabilityFraud                           | string            | `"0.0191140026"`         | Fraud probability score ranging from `0.0` to `1.0`.                                                                         |
| walletAddress                              | string            | `"0xa45f..."`            | The audited blockchain wallet address.                                                                                       |
| chain                                      | string            | `"ETH"`                  | The network/blockchain name (e.g., `"ETH"`).                                                                                 |
| lastChecked                                | string (ISO 8601) | `"2026-04-15T12:02:21Z"` | Timestamp of the most recent fraud analysis.                                                                                 |
| checked\_times                             | number            | `169`                    | Total number of times this address has been checked.                                                                         |
| createdAt                                  | string (ISO 8601) | `"2023-07-19T10:15:00Z"` | Record creation timestamp.                                                                                                   |
| updatedAt                                  | string (ISO 8601) | `"2026-05-08T14:30:22Z"` | Last update timestamp.                                                                                                       |
| forensic\_details                          | object            | _See sub-fields below_   | Contains 18 AML flags. Each is `"0"` (clean) or `"1"` (flagged). If any flag is `"1"`, `probabilityFraud` defaults to `1.0`. |
| ↳ `.cybercrime`                            | string            | `"0"`                    | Cybercrime activity flag.                                                                                                    |
| ↳ `.money_laundering`                      | string            | `"0"`                    | Money laundering flag.                                                                                                       |
| ↳ `.number_of_malicious_contracts_created` | string            | `"0"`                    | Creation of malicious smart contracts flag.                                                                                  |
| ↳ `.gas_abuse`                             | string            | `"0"`                    | Gas price manipulation behavior flag.                                                                                        |
| ↳ `.financial_crime`                       | string            | `"0"`                    | General financial crime flag.                                                                                                |
| ↳ `.darkweb_transactions`                  | string            | `"0"`                    | Transactions associated with darkweb marketplaces flag.                                                                      |
| ↳ `.reinit`                                | string            | `"0"`                    | Smart contract reinitialization abuse flag.                                                                                  |
| ↳ `.phishing_activities`                   | string            | `"0"`                    | Phishing vector participation flag.                                                                                          |
| ↳ `.fake_kyc`                              | string            | `"0"`                    | Use or association with fake KYC data flag.                                                                                  |
| ↳ `.blacklist_doubt`                       | string            | `"0"`                    | Association with blacklisted entities flag.                                                                                  |
| ↳ `.fake_standard_interface`               | string            | `"0"`                    | Spoofed/fake token interface usage flag.                                                                                     |
| ↳ `.stealing_attack`                       | string            | `"0"`                    | Active theft or exploit activity flag.                                                                                       |
| ↳ `.blackmail_activities`                  | string            | `"0"`                    | Blackmail or extortion activities flag.                                                                                      |
| ↳ `.sanctioned`                            | string            | `"0"`                    | Presence on known sanctions lists flag.                                                                                      |
| ↳ `.malicious_mining_activities`           | string            | `"0"`                    | Malicious or hijacked mining operations flag.                                                                                |
| ↳ `.mixer`                                 | string            | `"0"`                    | Interaction with crypto mixing services (e.g., Tornado Cash) flag.                                                           |
| ↳ `.fake_token`                            | string            | `"0"`                    | Creation or deployment of fake/honeypot tokens flag.                                                                         |
| ↳ `.honeypot_related_address`              | string            | `"0"`                    | Association with honeypot contract setups flag.                                                                              |
| sanctionData                               | array (object)    | _See sub-fields below_   | Definitive sanction matches associated with this address.                                                                    |
| ↳ `.isSanctioned`                          | boolean           | `false`                  | `true` if sanctioned, `false` if clean.                                                                                      |
| ↳ `.category`                              | string (nullable) | `"Terrorism"`            | Category of sanction if applicable; otherwise `null`.                                                                        |
| ↳ `.name`                                  | string (nullable) | `"Entity Name"`          | Official name of the sanctioned entity/person; otherwise `null`.                                                             |
| ↳ `.description`                           | string (nullable) | `"Details here..."`      | Contextual details regarding the sanction; otherwise `null`.                                                                 |
| ↳ `.url`                                   | string (nullable) | `"https://..."`          | Reference URL providing sanction evidence; otherwise `null`.                                                                 |

## Error Handling

| Status | Description                       |
| ------ | --------------------------------- |
| 400    | Invalid network or missing fields |
| 401    | Missing or invalid auth           |
| 402    | Insufficient balance              |
| 502    | Provider error                    |
