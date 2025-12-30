---
description: >-
  Example code for the web3_clientVersion JSON-RPC method. Complete guide on how
  to use web3_clientVersion JSON-RPC in GetBlock Web3 documentation.
---

# web3\_clientVersion - Base

The `web3_clientVersion` method returns the current client version. This is useful for identifying the node software and version being used to serve RPC requests.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_clientVersion",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'web3_clientVersion',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Client Version:', response.data.result);
```
{% endtab %}

{% tab title="Requests (Python)" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'web3_clientVersion',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
print(f'Client Version: {result["result"]}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "web3_clientVersion",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Client Version: {}", response["result"]);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "op-geth/v1.101308.2/linux-amd64/go1.21"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Client version string                   |

## Use Cases

* Node Identification: Identify the execution client being used
* Compatibility Check: Verify client version supports required features
* Debugging: Include client info in error reports
* Monitoring: Track client versions across infrastructure

## Error Handling

| Error Code | Message        | Description           |
| ---------- | -------------- | --------------------- |
| -32603     | Internal error | Node internal failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getClientVersion() {
    const version = await provider.send('web3_clientVersion', []);
    console.log('Client Version:', version);
    return version;
}

getClientVersion();
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getClientVersion() {
    const version = await client.request({
        method: 'web3_clientVersion',
        params: []
    });
    console.log('Client Version:', version);
    return version;
}

getClientVersion();
```
{% endtab %}
{% endtabs %}
