---
description: >-
  Example code for the rpc_modules json-rpc method. Сomplete guide on how to use
  rpc_modules json-rpc in GetBlock.io Web3 documentation.
---

# rpc\_modules - Arbitrum

This method returns the current network ID of the connected Ethereum-compatible chain. Each network has a unique numeric identifier, and this method helps applications determine which chain they are interacting with.&#x20;

#### Parameters

* None

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
    "method": "rpc_modules",
    "params": []
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "rpc_modules",
    "params": [],
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
   jsonrpc": "2.0",
    "method": "rpc_modules",
    "params": []
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
   jsonrpc": "2.0",
    "method": "rpc_modules",
    "params": []
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
    "result": {
        "arb": "1.0",
        "eth": "1.0",
        "net": "1.0",
        "rpc": "1.0",
        "txpool": "1.0",
        "web3": "1.0"
    }
}
```

#### Reponse Parameter Definition

| Field   | Type    | Description                                       |
| ------- | ------- | ------------------------------------------------- |
| jsonrpc | string  | JSON-RPC version.                                 |
| id      | integer | Request identifier.                               |
| result  | object  | Key-value list of supported modules and versions. |

#### Use case

The `rpc_modules` method helps developers to:

* Helps tooling determine what RPC namespaces the node supports.
* Blockchain tools use this to enable or disable features dynamically.
* Confirms that an RPC node exposes required modules.
* Ensures apps run only if the required modules are available.

#### Error handling

| Status Code | Error Message | Cause                             |
| ----------- | ------------- | --------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS\_TOKEN. |

#### Integration with Web3

Using `rpc_modules` allows developers to:

* Tools adapt automatically based on enabled modules.
* Ensures required RPC namespaces exist before dApp initialization.
* Helps compare feature sets across RPC providers.

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send( "rpc_modules",
[],);    
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
          method: "rpc_modules",
          params: [
          ],
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
