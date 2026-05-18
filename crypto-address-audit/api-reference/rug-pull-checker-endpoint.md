---
description: >-
  Example code for the /rug-pull/check method. Сomplete guide on how to use
  /rug-pull/check in GetBlock Address Audit documentation.
---

# Rug Pull Checker Endpoint

This method checks a smart contract for rug pull indicators. Proxied to ChainAware.

## Body Parameters

| Parameter         | Type    | Required | Description                                                   |
| ----------------- | ------- | -------- | ------------------------------------------------------------- |
| network           |  string | Yes      | The blockchain network to verify(ETH, BNB,POLY, TRON or BASE) |
| contract\_address | string  | Yes      | the contract address                                          |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://services.getblock.io/v1/rug-pull/check' \
-- header 'Authorization: Bearer YOUR_API_KEY',
--header 'Content-Type: application/json' \
--data-raw ' {  "network": "ETH", "address": "0xdac17f958d2ee523a2206206994597c13d831ec7"}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from "axios";

const data = 
  {  "network": "ETH", "address": "0xdac17f958d2ee523a2206206994597c13d831ec7"};

const config = {
  method: "post",
  url: "https://services.getblock.io/v1/rug-pull/check",
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
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://services.getblock.io/v1/rug-pull/check"

payload = json.dumps({  "network": "ETH", "address": "0xdac17f958d2ee523a2206206994597c13d831ec7"})

headers = {
      "Authorization": "Bearer YOUR_API_KEY",
  "Content-Type": "application/json",
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust (Reqwest)" %}
{% code overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({  "network": "ETH", "address": "0xdac17f958d2ee523a2206206994597c13d831ec7"});

    let response = client
        .post("https://services.getblock.io/v1/rug-pull/check")
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "data": {
        "message": "Success",
        "contractAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "pairAddress": null,
        "contractCreatorAddress": "0x36928500bc1dcd7af6a2b4008875cc336b927d57",
        "risk_score": 0,
        "risk_status": null,
        "risk_indicators": {},
        "status": "Not Fraud",
        "probabilityFraud": "0.0028322712",
        "chain": "ETH",
        "lastChecked": "2026-05-14T14:23:18.000Z",
        "contractCreationTime": "2017-11-28T00:41:21.000Z",
        "forensic_details": {
            "owner": {
                "owner_address": "0xc6cde7c39eb2f0f0095f41570af89efc2c1ea828",
                "owner_name": "owner",
                "owner_type": "contract"
            },
            "privilege_withdraw": 0,
            "withdraw_missing": 0,
            "is_open_source": 1,
            "blacklist": 1,
            "contract_name": "TetherToken",
            "selfdestruct": 0,
            "is_proxy": 0,
            "approval_abuse": 0
        },
        "checked_times": 1,
        "createdAt": "2025-09-04T04:55:17.000Z",
        "updatedAt": "2026-05-14T14:23:20.000Z"
    }
}
```
{% endcode %}

## Response Parameters



## Error Handling

| Status | Description                       |
| ------ | --------------------------------- |
| 400    | Invalid network or missing fields |
| 401    | Missing or invalid auth           |
| 402    | Insufficient balance              |
| 502    | Provider error                    |
