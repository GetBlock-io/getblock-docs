---
description: >-
  Example code for the eth_getBalance json-rpc method. Сomplete guide on how to
  use eth_getBalance json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBalance - Arbitrum

This method retrieves the account balance of a given address. The balance is returned in **wei**, the smallest unit of Ether.

#### Parameters

| Parameter | Type   | Required | Description                                                                                       |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------- |
| address   | string | yes      | The account address to query (0x-prefixed).                                                       |
| block     | string | yes      | Block number or tag. Can be: `"latest"`, `"earliest"`, `"pending"` or a hex-encoded block number. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ]
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
};

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
  .then((response) => {
    console.log(JSON.stringify(response.data));
  })
  .catch((error) => {
    console.log(error);
  });

```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}

{% tab title="Rust" %}
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "id": "getblock.io",
  "params": [
        "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "latest"
    ],
}"#;

    let json: serde_json::Value = serde_json::from_str(&data)?;

    let request = client.request(reqwest::Method::POST, "https://go.getblock.us/<ACCESS_TOKEN>")
        .headers(headers)
        .json(&json);

    let response = request.send().await?;
    let body = response.text().await?;

    println!("{}", body);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x20a2c3a842980"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                |
| ------ | ------------ | -------------------------- |
| result | string (hex) | Account balance in **wei** |

#### Use case

`eth_getBalance` is used to:

* Display a user's wallet balance in dApps and wallets
* Check on-chain balances for dashboards or explorers
* Validate whether an account can pay gas fees
* Automated scripts can monitor whale wallets or treasury wallets
* DeFi apps check collateral levels or borrowing capacity

#### Error handling

| Status Code | Error Message    | Cause                                       |
| ----------- | ---------------- | ------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.           |
| -32602      | Invalid argument | Wallet address isn't accurate or incomplete |

#### Integration with Web3

The `eth_getBalance` can help developers:

* Fetch balances dynamically without requiring a transaction
* Power trustless dashboards and explorers
* Validate user accounts before executing transactions
* Track portfolio values in real time
* Access historical balances using specific block numbers

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBalance", ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
```
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from 'viem';
import { arbitrum } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: arbitrum,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_getBalance',
            params: ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endtab %}
{% endtabs %}
