---
description: >-
  Example code for the eth_accounts json-rpc method. Сomplete guide on how to
  use eth_accounts json-rpc in GetBlock.io Web3 documentation.
---

# eth\_accounts - Optimism

This method  is used to retrieve addresses that the node controls.

{% hint style="info" %}
&#x20;In the context of GetBlock's infrastructure, since no private keys are stored on the nodes, this method will return an empty array. It's primarily useful for compatibility with tools that expect this endpoint to exist.
{% endhint %}

### Parameters

* None

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Python(Request)" %}
{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_accounts result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_accounts",
    params: [],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_accounts result:", response.data.result);
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

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
    "jsonrpc": "2.0",
     "method": "eth_accounts",
    "params": [],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": []
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: An empty array, as GetBlock nodes do not store private keys.

### Use Case

The eth\_accounts method is commonly used for:

* Web3 provider initialization
* Ethereum tool compatibility
* Client configuration checks

#### Error handling

<table><thead><tr><th>Status Code</th><th>Error Message</th><th>Cause</th></tr></thead><tbody><tr><td>403</td><td><pre><code>RBAC: access denied
</code></pre></td><td>Missing or invalid ACCESS_TOKEN.</td></tr></tbody></table>

#### Integration with Web3

The `eth_accounts` can help developers to:

* Identify available signing accounts
* Auto-select a default wallet in development
* Validate that a local signer exists
* Simplify onboarding in testing environments

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

// Initialize Ethers provider with GetBlock
const provider = new ethers.JsonRpcProvider(
    'https://go.getblock.us/<ACCESS-TOKEN>/'
);

// Using the method through Ethers
async function useEthersMethod() {
    try {
        // Method-specific Ethers implementation
        const result = await provider.send('eth_accounts', []);
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Ethers Error:', error);
        throw error;
    }
}
```
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```js
import { useEffect, useState } from "react";
import { createPublicClient, http } from "viem";
import { optimism } from "viem/chains";
// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http("https://go.getblock.io/<ACCESS_TOKEN>"),
});
// Using the method through Viem
async function Call() {
  try {
 const result = await client.request({
   method: "eth_accounts",
   params: [
   ],
 });
 console.log("Current Block:", result);
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
