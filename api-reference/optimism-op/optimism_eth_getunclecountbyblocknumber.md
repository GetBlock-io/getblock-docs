---
description: >-
  Example code for the eth_getUncleCountByBlockNumber json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockNumber json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleCountByBlockNumber - Optimism

This method returns the number of uncle blocks in a specified block by block number. On Optimism, this always returns 0 since uncles don't exist in the rollup model.

### Parameters

| Parameter   | Type          | Description                                              | Required |
| ----------- | ------------- | -------------------------------------------------------- | -------- |
| blockNumber | QUANTITY\|TAG | Block number in hex, or 'latest', 'earliest', 'pending'. | Yes      |

### Request Sample

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

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The uncle count as hexadecimal (always 0x0 on Optimism).

### Use Case

The eth\_getUncleCountByBlockNumber method is commonly used for:

* **Ethereum compatibility**
* **Block structure analysis**

#### Error handling

| Status Code | Error Message    | Cause                                  |
| ----------- | ---------------- | -------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.      |
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
