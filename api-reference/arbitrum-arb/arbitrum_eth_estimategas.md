---
description: >-
  Example code for the eth_estimateGas json-rpc method. Сomplete guide on how to
  use eth_estimateGas json-rpc in GetBlock.io Web3 documentation.
---

# eth\_estimateGas - Arbitrum

This method estimates the gas required to execute a transaction on Arbitrum without broadcasting it.\
This simulates execution and returns the minimal gas needed.

#### Parameters

| Field       | Type          | Required | Description                                                   |
| ----------- | ------------- | -------- | ------------------------------------------------------------- |
| transaction | object        | yes      | Main transaction object used for estimation                   |
| from        | string        | optional | Sender address. Required for some contract calls              |
| to          | string        | optional | Recipient or contract address. Omit when deploying a contract |
| gas         | string (hex)  | optional | Gas limit upper bound; node adjusts actual estimation         |
| gasPrice    | string (hex)  | optional | Optional gas price included for compatibility                 |
| value       | string (hex)  | optional | Amount of ETH sent with the transaction                       |
| data        | string        | optional | Encoded function call or contract bytecode                    |
| block tag   | string or hex | optional | Block context for the estimation, default is latest           |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_estimateGas",
  "id": "getblock.io",
  "params": [
    {
      "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
      "value": "0x1"
    }
  ],
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "eth_estimateGas",
  "id": "getblock.io",
  "params": [
    {
      "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
      "value": "0x1"
    }
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
  "method": "eth_estimateGas",
  "id": "getblock.io",
  "params": [
    {
      "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
      "value": "0x1"
    }
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
  "method": "eth_estimateGas",
  "id": "getblock.io",
  "params": [
    {
      "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "to": "0x44aa93095d6749a706051658b970b941c72c1d53",
      "value": "0x1"
    }
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
    "result": "0x5208"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                |
| ------ | ------------ | ------------------------------------------ |
| result | string (hex) | Estimated gas required for the transaction |

#### Use case

`eth_estimateGas` is used to:

* Estimating gas before sending an on-chain transaction
* Setting safe gas limits for dApps and wallets
* Contract deployments
* Approvals, swaps, mints, transfers
* Estimating costs for batch calls
* Preventing out-of-gas failures

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_estimateGas` can help developers:

* Allows dApps to simulate actions safely
* Helps builders understand live contract state without spending gas
* Useful for DeFi dashboards, explorers, and analytics tools
* Enables wallets to preview costs before sending a transaction
* Supports trustless frontends because the estimation is done by the blockchain directly
* Test interactions like swaps, mints, or transfers without actual execution

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_estimateGas", [
       {
         from: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
         to: "0x44aa93095d6749a706051658b970b941c72c1d53",
         value: "0x1",
       },
     ]);
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
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>");

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
          method: "eth_estimateGas",
          params: [
            {
              from: "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
              to: "0x44aa93095d6749a706051658b970b941c72c1d53",
              value: "0x1",
            },
          ]
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
