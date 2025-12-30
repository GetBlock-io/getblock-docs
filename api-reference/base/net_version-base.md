---
description: >-
  Example code for the net_version JSON-RPC method. Complete guide on how to use
  net_version JSON-RPC in GetBlock Web3 documentation.
---

# net\_version - Base

The `net_version` method returns the current network ID. For Base mainnet, this returns "8453". This method is primarily used for legacy compatibility, as `eth_chainId` is the preferred method for modern applications.

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
    "method": "net_version",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'net_version',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Network ID:', response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'net_version',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
print(f'Network ID: {result["result"]}')
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
            "method": "net_version",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Network ID: {}", response["result"]);
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
    "result": "8453"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Network ID as a decimal string          |

## Use Cases

* Network Verification: Confirm connection to expected network
* Legacy Compatibility: Support older wallet integrations
* Multi-network Apps: Route connections to correct network
* Configuration Validation: Verify RPC endpoint configuration

## Error Handling

| Error Code | Message        | Description           |
| ---------- | -------------- | --------------------- |
| -32603     | Internal error | Node internal failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getNetworkId() {
    const network = await provider.getNetwork();
    console.log('Network ID:', network.chainId);
    console.log('Network Name:', network.name);
    return network;
}

getNetworkId();
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

async function getNetworkId() {
    const chainId = await client.getChainId();
    console.log('Chain/Network ID:', chainId);
    return chainId;
}

getNetworkId();
```
{% endtab %}
{% endtabs %}
