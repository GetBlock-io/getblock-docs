---
description: >-
  Example code for the eth_getTransactionCount json-rpc method. Сomplete guide
  on how to use eth_getTransactionCount json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionCount - Optimism

This method returns the number of transactions sent from a specific address, which is also known as the nonce. The nonce is critical for transaction ordering and preventing replay attacks. When sending transactions, you need to use the correct nonce value. Using `'pending'` as the block parameter returns the nonce including pending transactions, which is essential for sending multiple transactions in sequence.

### Parameters

| Parameter | Type           | Description                                                    | Required |
| --------- | -------------- | -------------------------------------------------------------- | -------- |
| address   | DATA, 20 Bytes | The address to get the transaction count for.                  | Yes      |
| block     | QUANTITY\|TAG  | Block number in hex, or `'latest'`, `'earliest'`, `'pending'`. | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0xCDe377407d9b819EFc0e185Aa2D442194c2233E6", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(Axios)" %}
{% code overflow="wrap" %}
```bash
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0xCDe377407d9b819EFc0e185Aa2D442194c2233E6", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionCount result:", result)
else:
    print("Error:", response.status_code, response.text);

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionCount result:", response.data.result);
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
    "method": "eth_getTransactionCount",
    "params": ["0xCDe377407d9b819EFc0e185Aa2D442194c2233E6", "latest"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionCount result:", result)
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
   "method": "eth_getTransactionCount",
    "params": [
        "0xCDe377407d9b819EFc0e185Aa2D442194c2233E6",
        "latest"
    ],
    "id": "getblock.io"
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
    "result": "0xb1fddc"
}
```

### Response Parameters

| Field  | Type         | Description                                                                     |
| ------ | ------------ | ------------------------------------------------------------------------------- |
| result | string (hex) | Number of transactions sent from this address. Equivalent to the account nonce. |

### Use Case

The eth\_getTransactionCount method is commonly used for:

* **Transaction nonce management**
* **Account activity analysis**
* **Transaction sequencing**
* **Wallet implementation**

#### Error handling

| Status Code | Error Message    | Cause                                                                    |
| ----------- | ---------------- | ------------------------------------------------------------------------ |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.                                        |
| -32602      | Invalid argument | <ul><li>Invalid address</li><li>Block parameter not recognized</li></ul> |

#### Integration with Web3

The `eth_getTransactionCount` method helps developers to:

* Manage nonces automatically when sending transactions
* Prevent duplicate transactions by checking the latest nonce
* Build reliable wallet transaction pipelines
* Track account activity in explorers and dashboards
* Detect pending transactions by comparing `pending` vs `latest` values
* Support batch transaction systems that depend on precise nonce control

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getTransactionByHash",
    [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getTransactionCount",
      params: ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"]});
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
