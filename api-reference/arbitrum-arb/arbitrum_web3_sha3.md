---
description: >-
  Example code for the web3_sha3 JSON RPC method. Сomplete guide on how to use
  web3_sha3 JSON RPC  in GetBlock Web3 documentation.
---

# web3\_sha3 - Arbitrum

This method returns Keccak-256 (not the standardized SHA3-256) hash of the given data.

#### Parameters

| parameters | Data Type | description                                        |
| ---------- | --------- | -------------------------------------------------- |
| data       | string    | The data to hash, encoded as a hexadecimal string. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "web3_sha3",
  "params": ["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "web3_sha3",
  "params": ["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"],
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
  "method": "web3_sha3",
  "params": ["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"],
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
  "method": "web3_sha3",
  "params": ["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"],
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
    "result": "0xf5d3389b32cee33308ce9af2d8aefa1517aa90ccb7b1d2eb29c61f13e1fd3cea"
}
```

#### Reponse Parameter Definition

| Field  | Type   | Description                                                     |
| ------ | ------ | --------------------------------------------------------------- |
| result | string | The Keccak-256 hash of the input data, as a hexadecimal string. |

#### Use case

The `web3_sha3` method helps developers to:

* Generate a hash of arbitrary data for off-chain validation.
* Create message digests for signing and verification.
* Derive storage keys for smart contracts.
* Hash inputs for deterministic contract logic or mapping keys.
* Prepare data for signature-based login flows in dApps.

#### Error handling

| Status Code | Error Message    | Cause                              |
| ----------- | ---------------- | ---------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.  |
| -32602      | invalid argument | Input is not properly hex-encoded. |

#### Integration with Web3

Using `web3_sha3` allows developers to:

* Ensure consistent hashing for signing or contract storage.
* Generate identifiers or keys deterministically.
* Validate data integrity off-chain.
* Prepare inputs for smart contracts that require Keccak-256 hashes.
* Build signature-based authentication or meta-transaction systems.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("web3_sha3",
["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"]);    
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
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "web3_sha3",
         params: ["0xf2a2b854721d4372474fc76cc13445a73369c0c334f4935c88bde3c310f28c9a"],
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
