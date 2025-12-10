---
description: >-
  Example code for the eth_getCode json-rpc method. Сomplete guide on how to use
  eth_getCode json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getCode - Arbitrum

This method returns the bytecode stored at a specific address. If the address has no smart contract deployed, the result will be `0x`.

#### Parameters

| Parameter     | Type   | Required | Description                                                                              |
| ------------- | ------ | -------- | ---------------------------------------------------------------------------------------- |
| address       | string | yes      | The target address to inspect. Must be a valid 20 byte Ethereum-style address.           |
| block\_number | string | yes      | The block number or state override reference such as `latest`, `earliest`, or `pending`. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getCode",
    "params": [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
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
   "method": "eth_getCode",
    "params": [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
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
    "method": "eth_getCode",
    "params": [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
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
   "method": "eth_getCode",
    "params": [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
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
    "result": "0x"
}
```

#### Reponse Parameter Definition

| Field  | Type         | Description                                                                            |
| ------ | ------------ | -------------------------------------------------------------------------------------- |
| result | string (hex) | Contract bytecode at the given address. Returns `0x` if the address is not a contract. |

#### Use case

The `eth_getCode` is used to:

* Checking if an address is a contract or an externally owned account
* Verifying onchain deployments for security audits
* Allowing frontends to detect contract upgrades by comparing bytecode
* Supporting explorers that display contract metadata
* Identifying proxy contracts by checking for minimal bytecode patterns

#### Error handling

| Status Code | Error Message    | Cause                                                                                                                                 |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                                                                     |
| -32602      | Invalid argument | <ul><li>The Address not 20 bytes or missing prefix</li><li>block hash isn't accurate or incomplete </li><li>keyword missing</li></ul> |

#### Integration with Web3

The `eth_getCode` method helps developers to:

* Detect whether a user is interacting with a contract or a wallet
* Validate deployed bytecode for audit and debugging purposes
* Build tools that identify proxies, multicall contracts, routers, and libraries
* Support trustless dapps that rely on on-chain introspection
* Ensure safe interactions by verifying contract code before sending transactions

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getCode",
  [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
        "latest"
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
          "method": "eth_getCode",
    "params": [
        "0x9b956e3d318625be2686ae7268d81777c462d41f",
        "latest"
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
