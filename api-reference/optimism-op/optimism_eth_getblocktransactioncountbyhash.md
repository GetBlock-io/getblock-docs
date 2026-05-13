---
description: >-
  Example code for the eth_getBlockTransactionCountByHash json-rpc method.
  Сomplete guide on how to use eth_getBlockTransactionCountByHash json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - Optimism

This method retrieves the number of transactions in a block using its hash. It's useful for quickly determining block activity without fetching the full block data. On Optimism, blocks typically contain fewer transactions than Ethereum L1 due to the faster block time (\~2 seconds).

### Parameters

| Parameter | Type           | Description          | Required |
| --------- | -------------- | -------------------- | -------- |
| blockHash | DATA, 32 Bytes | The hash of a block. | Yes      |

### Request Sample

{% tabs %}
{% tab title="curl" %}
```bash
{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByHash",
"params": ["0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"], 
"id": "getblock.io"
}
```
{% endtab %}

{% tab title="Javascript(Axios)" %}
```bash
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
   "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"
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

{% tab title="Python(Request)" %}
{% code overflow="wrap" %}
```py
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
   "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"
    ],
    "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

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
   "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"
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
    "result": "0x28"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The number of transactions in the block as a hexadecimal string.&#x20;

### Use Case

The eth\_getBlockTransactionCountByHash method is commonly used for:

* **Block analysis**
* **Transaction throughput monitoring**
* **Network activity tracking**

#### Error handling

| Status Code | Error Message    | Cause                                        |
| ----------- | ---------------- | -------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.            |
| -32602      | Invalid argument | The block hash isn't accurate or incomplete  |

#### Integration with Web3

The `eth_getBlockTransactionCountByHash` can help developers:

* Power block explorers that show transaction counts without loading full blocks
* Measure chain activity for dashboards and metrics
* Build fast monitoring tools that track throughput
* Reduce bandwidth usage by avoiding heavy block queries
* Support systems that decide whether to fetch full block data based on count

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getBlockTransactionCountByHash",
["0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"]);    
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
{% code overflow="wrap" %}
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
           method:   "eth_getBlockTransactionCountByHash",
           params:   ["0xe8f38578b756a9fd05ca05dcbe56fd57ce72e9fb06a90e5064562baccf7c7a6e"]
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
