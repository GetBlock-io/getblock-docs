---
description: >-
  Example code for the eth_estimateGas json-rpc method. Сomplete guide on how to
  use eth_estimateGas json-rpc in GetBlock.io Web3 documentation.
---

# eth\_estimateGas - Optimism

This method simulates a transaction and returns an estimate of the L2 gas that would be consumed. The estimate may be higher than the actual gas used because it accounts for the worst-case execution path.&#x20;

{% hint style="info" %}
On Optimism, this only estimates L2 execution gas. The total transaction cost also includes L1 data fees. For a complete cost estimate, use the GasPriceOracle contract or the Optimism SDK.
{% endhint %}

### Parameters

| Parameter   | Type          | Description                                                                                                                                     | Required |
| ----------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| transaction | Object        | Transaction call object with from (optional), to (optional), gas (optional), gasPrice (optional), value (optional), and data (optional) fields. | Yes      |
| block       | QUANTITY\|TAG | (Optional) Block number in hex, or 'latest', 'earliest', 'pending'.                                                                             | No       |

### Request. Sample

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", 
                "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Javascript(Axios)" %}
{% code overflow="wrap" %}
```bash
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_estimateGas",
    params: [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_estimateGas result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endcode %}
{% endtab %}

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f7bD5e", "to": "0x8ba1f109551bD432803012645Ac136dff489ca2c", "value": "0xde0b6b3a7640000"}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_estimateGas result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The estimated L2 gas amount as a hexadecimal string. `0x5208` equals 21,000 gas (standard ETH transfer).

### Use Case

The eth\_estimateGas method is commonly used for:

* **Transaction cost estimation**
* **Gas limit setting**
* **Pre-transaction validation**
* **DApp UX optimization**

#### Error handling

| Status Code | Error Message                   | Cause                             |
| ----------- | ------------------------------- | --------------------------------- |
| 404         | Not Found                       | Missing or invalid ACCESS\_TOKEN. |
| -32000      | insufficient funds for transfer |                                   |

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

{% tab title="View" %}
{% code overflow="wrap" %}
```js
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
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
{% endcode %}
{% endtab %}
{% endtabs %}
