---
description: >-
  Example code for the eth_gasPrice json-rpc method. Сomplete guide on how to
  use eth_gasPrice json-rpc in GetBlock.io Web3 documentation.
---

# eth\_gasPrice - Arbitrum

This method gets the current average gas price on the Arbitrum network.\
This value is derived from recent transactions and can be used as a baseline for setting transaction fees.

#### Parameters

* None

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_gasPrice",
  "id": "getblock.io",
  "params": [],
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_gasPrice",
  "id": "getblock.io",
  "params": [],
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
  "method": "eth_gasPrice",
  "id": "getblock.io",
  "params": [],
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
  "method": "eth_gasPrice",
  "id": "getblock.io",
  "params": [],
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
    "result": "0x989680"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                      |
| ------ | ------------ | -------------------------------- |
| result | string (hex) | Current average gas price in wei |

#### Use case

`eth_gasPrice` is used to:

* Determines the baseline cost for sending transactions
* Allows wallets to calculate estimated fees
* Helps dApps display up-to-date network fees
* Useful for automated bots adjusting to network congestion
* Let's developers optimize cost-sensitive interactions such as bridging, swapping, or minting

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_gasPrice` can help developers:

* Display real-time gas prices in interfaces
* Adjust transaction fees automatically
* Improve user experience during times of congestion
* Support trustless fee estimation without relying on third-party APIs

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_gasPrice", []);
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
            method: 'eth_gasPrice',
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
