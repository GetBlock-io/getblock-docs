---
description: >-
  Example code for the eth_getUncleCountByBlockNumber JSON RPC method. Сomplete
  guide on how to use eth_getUncleCountByBlockNumber JSON RPC  in GetBlock Web3
  documentation.
---

# eth\_getUncleCountByBlockNumber - Arbitrum

This method returns the number of uncles in a block specified by its block number.

{% hint style="warning" %}
Arbitrum does not produce uncle blocks, so this method always returns **"0x0"**, but it remains available for full Ethereum RPC compatibility.
{% endhint %}

#### Parameters

| Parameter     | Type   | Required | Description                                                                                                       |
| ------------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------- |
| block\_number | string | yes      | The block number in hex (for example `"0x1a"`), or predefined tags like `"latest"`, `"earliest"`, or `"pending"`. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "eth_getUncleCountByBlockNumber",
  "params": [
    "latest"
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
     "method": "eth_getUncleCountByBlockNumber",
  "params": [
    "latest"
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
     "method": "eth_getUncleCountByBlockNumber",
  "params": [
    "latest"
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
     "method": "eth_getUncleCountByBlockNumber",
  "params": [
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

#### Reponse Parameter Definition

| Field  | Description                                         | Data Type |
| ------ | --------------------------------------------------- | --------- |
| result | The number of uncle blocks as a hexadecimal string. | String    |

#### Use case

Although Arbitrum never returns a nonzero value, this method helps developers to:

* Maintain multi-chain compatibility with Ethereum RPC methods
* Prevent breaking explorers or indexers that expect the method
* Use the same codebase across EVM chains without conditionals
* Ensure analytics tools can safely query block structure

#### Error handling

| Status Code | Error Message    | Cause                                  |
| ----------- | ---------------- | -------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.      |
| -32602      | Invalid argument | <ul><li>Invalid block number</li></ul> |

#### Integration with Web3

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getUncleCountByBlockNumber",
 [
    "latest"
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
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getUncleCountByBlockNumber",
         params: ["latest"],
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
