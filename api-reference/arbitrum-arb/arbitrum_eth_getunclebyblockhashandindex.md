---
description: >-
  Example code for the eth_getUncleByBlockHashAndIndex JSON RPC method. Сomplete
  guide on how to use eth_getUncleByBlockHashAndIndex JSON RPC  in GetBlock Web3
  documentation.
---

# eth\_getUncleByBlockHashAndIndex - Arbitrum

This method returns information about an uncle block by referencing the parent block's hash and the uncle's index.

{% hint style="warning" %}
Since Arbitrum does not produce uncle blocks, this method always returns **null**, but it is still included for EVM compatibility.
{% endhint %}

#### Parameters

| Parameter    | Type   | Required | Description                                                                                  |
| ------------ | ------ | -------- | -------------------------------------------------------------------------------------------- |
| block\_hash  | string | yes      | The hash of the block containing the uncle. Must be a 32 byte hex string starting with `0x`. |
| uncle\_index | string | yes      | The index position of the uncle block. Must be a hex number, for example `"0x0"`.            |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
  "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
  ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
     "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
  ],
    "id": "getblock.io"
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
    "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
  ],
    "id": "getblock.io"
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
   "method": "eth_getUncleByBlockHashAndIndex",
  "params": [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": null
}
```

#### Reponse Parameter Definition

| Field      | Description                             |
| ---------- | --------------------------------------- |
| number     | Block number of the uncle               |
| hash       | Hash of the uncle block                 |
| parentHash | Hash of parent block                    |
| miner      | Address of miner who produced the uncle |
| difficulty | Difficulty of the uncle block           |
| gasLimit   | Gas limit                               |
| gasUsed    | Gas used                                |
| timestamp  | Timestamp of the uncle block            |
| uncles     | Always empty for uncles                 |

{% hint style="info" %}
But again, on Arbitrum the value is always **null**.
{% endhint %}

#### Use case

Even though Arbitrum does not generate uncle blocks, developers may still call this method because it helps to:

* Maintain compatibility with Ethereum-based tools, explorers, and SDKs
* Avoid breaking code paths where multiple RPCs are used generically
* Simplify integration with indexing libraries that expect uncle queries

#### Error handling

| Status Code | Error Message    | Cause                                                      |
| ----------- | ---------------- | ---------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                          |
| -32602      | Invalid argument | <ul><li>Invalid block hash</li><li>Invalid index</li></ul> |

#### Integration with Web3

The `eth_getUncleByBlockHashAndIndex` method helps developers to:

* Maintain uniform code paths when interacting with multiple EVM networks
* Ensure compatibility with Ethereum-based frameworks
* Safely account for uncle data even when none exist
* Support explorers, analytics platforms, and monitoring tools needing consistent RPC coverage

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getUncleByBlockHashAndIndex", [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
  ],);    
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
         method: "eth_getUncleByBlockHashAndIndex",
         params: [
    "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "0x0"
  ]});
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
