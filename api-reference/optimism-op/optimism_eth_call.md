---
description: >-
  Example code for the eth_call json-rpc method. Сomplete guide on how to use
  eth_call json-rpc in GetBlock.io Web3 documentation.
---

# eth\_call - Optimism

This method allows you to interact with smart contracts without sending a transaction or spending gas. It's commonly used to read data from contracts, such as checking token balances or calling view functions.&#x20;

{% hint style="success" %}
The **eth\_call** method executes a read-only call to a smart contract on Optimism without consuming gas or modifying state.
{% endhint %}

### Parameters

| Parameter   | Type            | Description                                                                                                               | Required |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------- | -------- |
| transaction | Object          | Transaction call object with from (optional), to, gas (optional), gasPrice (optional), value (optional), and data fields. | Yes      |
| block       | `QUANTITY\|TAG` | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`.                                                            | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(axios)" %}
{% code overflow="wrap" %}
```js
import axios from 'axios';
const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_call",
    params: [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_call result:", response.data.result);
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
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f7bD5e"}, "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_call result:", result)
else:
    print("Error:", response.status_code, response.text)
```
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

    let data = r#"{{
  "jsonrpc": "2.0",
  "method": "eth_call",
  "id": "getblock.io",
  "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
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
    "result": "0x000000000000000000000000ec23cf5a1db3dcc6595385d28b2a4d9b52503be4"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The return value of the executed contract call as a hexadecimal string.

### Use Case

The eth\_call method is commonly used for:

* **Reading token balances**
* **Calling view functions**
* **Contract state inspection**
* **Simulating transactions**

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 404         | Not found     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

The `eth_call` can help developers to:

* Enables trustless frontends
* Reads live contract state without gas
* Supports dashboards, DeFi analytics, wallets, NFT explorers
* Let's developers simulate transactions before execution

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/24915a36b17143a891d3b10447615258";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
     const result = await provider.send("eth_call", [
       {
         to: "0x1F98431c8aD98523631AE4a59f267346ea31F984",
         data: "0x8da5cb5b",
       },
       "latest",
     ]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("Ethers Error fetching block number:", error);
    throw error;
  }
}

```
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { useEffect, useState } from "react";
import { createPublicClient, http } from "viem";
import { optimism } from "viem/chains";


// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http("https://go.getblock.io/<ACCESS_TOKEN>
});
// Using the method through Viem
async function Call() {
  try {
 const result = await client.request({
   method: "eth_call",
   "params": [
    {
      "to": "0x1F98431c8aD98523631AE4a59f267346ea31F984",
      "data": "0x8da5cb5b"
    },
    "latest"
  ]
 });
 console.log("Response:", result);
 return result;
  }
   catch (error) {
 console.error("Viem Error:", error);
 throw error;
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
