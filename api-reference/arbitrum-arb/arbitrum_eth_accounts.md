---
description: >-
  Example code for the eth_accounts json-rpc method. Сomplete guide on how to
  use eth_accounts json-rpc in GetBlock.io Web3 documentation.
---

# eth\_accounts - Arbitrum

This method gets a list of addresses owned by the client.&#x20;

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
  "method": "eth_accounts",
  "params": [],
  "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
  jsonrpc: "2.0",
  method: "eth_accounts",
  params: [],
  id: "getblock.io",
});

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
  "method": "eth_accounts",
  "params": [],
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": [
    "0xd1f5279be4b4dd94133a23dee1b23f5bfe436tf3r"
  ]
}
```

#### Response Parameter Definition

| Field    | Data Type | Definition                                                                                                                         |
| -------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `result` | string    | <p>An array of addresses owned by the client. </p><p></p><p>If the accounts doesn't have any address, it returns empty array. </p> |

#### Use case

This method is generally used to:

* Retrieve the list of locally unlocked accounts on a full node.
* Identify which account a node can use for signing transactions.
* Pre–Web3 wallet integrations where the node itself managed keys.

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
const { ethers } = require('ethers');

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
{% endtabs %}
