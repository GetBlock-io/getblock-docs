---
description: >-
  Example code for the eth_getUncleByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getUncleByBlockNumberAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getUncleByBlockNumberAndIndex - Arbitrum

This method returns information about an uncle block using the parent block hash and the uncle index.

{% hint style="warning" %}
Arbitrum does not produce uncle blocks, so this method always returns **null**, but it exists for full EVM compatibility.
{% endhint %}

#### Parameters

| Parameter    | Type   | Required | Description                                                                                            |
| ------------ | ------ | -------- | ------------------------------------------------------------------------------------------------------ |
| block\_hash  | string | yes      | The hash of the block whose uncle is being requested. Must be a 32 byte hex string prefixed with `0x`. |
| uncle\_index | string | yes      | The index of the uncle in the block. Must be a hex number like `"0x0"`.                                |

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
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
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
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
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
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
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
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
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

| Field      | Description               |
| ---------- | ------------------------- |
| number     | Block number of the uncle |
| hash       | Hash of the uncle block   |
| parentHash | Parent block hash         |
| miner      | Miner address             |
| difficulty | Difficulty of uncle block |
| gasLimit   | Gas limit                 |
| gasUsed    | Gas used                  |
| timestamp  | Timestamp                 |
| uncles     | Always empty              |

{% hint style="info" %}
But again, on Arbitrum the value is always **null**.
{% endhint %}

#### Use case

Even though Arbitrum does not have uncle blocks, the method helps developers by:

* Maintaining compatibility with Ethereum RPC tooling
* Supporting indexers that expect standard EVM methods
* Allowing multi-chain dashboards to run a single RPC workflow
* Preventing errors in systems that automatically query all uncle endpoints

#### Error handling

| Status Code | Error Message    | Cause                                                      |
| ----------- | ---------------- | ---------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                          |
| -32602      | Invalid argument | <ul><li>Invalid block hash</li><li>Invalid index</li></ul> |

#### Integration with Web3

The `eth_getUncleByBlockHashAndIndex` method helps developers to:

* Build tools compatible with Ethereum and Arbitrum
* Implement universal RPC logic across multiple chains
* Handle uncle queries gracefully even when none exist
* Prevent backend crashes due to missing RPC behavior

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getUncleByBlockHashAndIndex",
  [
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
    "0x0"
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
         method: "eth_getUncleByBlockHashAndIndex",
         params: [
    "0x4cc1439cb36e8e984ed8c3562b222db19fac331ab79d9e90dc351325289621ea",
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
