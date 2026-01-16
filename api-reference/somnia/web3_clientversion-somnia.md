---
description: >-
  Example code for the web3_clientVersion JSON-RPC method. Complete guide on how
  to use web3_clientVersion JSON-RPC in GetBlock Web3 documentation
---

# web3\_clientVersion - Somnia

This method returns the current client version of the Somnia node. This is useful for debugging, compatibility checks, and verifying the node implementation.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "web3_clientVersion",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'web3_clientVersion',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "web3_clientVersion",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "web3_clientVersion",
        "params": []
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "Somnia/v1.0.0/linux-amd64"
}
```

## Response Parameters

| Parameter | Type   | Description                        |
| --------- | ------ | ---------------------------------- |
| result    | string | Client name, version, and platform |

## Use Cases

* Verify node version
* Debug compatibility issues
* Monitor node deployments
* Check for required features

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const version = await provider.send('web3_clientVersion', []);
console.log(version);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const version = await client.request({
  method: 'web3_clientVersion',
  params: []
});
console.log(version);
```
{% endcode %}
{% endtab %}
{% endtabs %}
