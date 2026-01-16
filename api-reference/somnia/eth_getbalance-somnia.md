---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide on how to
  use eth_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - Somnia

This method returns the balance of the account at a given address on the Somnia network. The balance is returned in wei (the smallest unit of SOMI, where 1 SOMI = 10^18 wei). This method is essential for wallet applications, balance verification, and transaction preparation.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The address to check balance (20 bytes, 0x-prefixed)    |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getBalance",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "latest"
  ]
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
  method: 'eth_getBalance',
  params: [
    '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
    'latest'
  ]
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
    "method": "eth_getBalance",
    "params": [
        "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
        "latest"
    ]
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
        "method": "eth_getBalance",
        "params": [
            "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
            "latest"
        ]
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

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x8ac7230489e80000"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| result    | string | Balance in wei as hex (0x8ac7230489e80000 = 10 SOMI) |

## Use Cases

* Display wallet balance in dApps
* Verify sufficient funds before transactions
* Check gas availability for operations
* Monitor account holdings
* Calculate portfolio values

## Error Handling

| Error Code | Description                                           |
| ---------- | ----------------------------------------------------- |
| -32602     | Invalid params - malformed address or block parameter |
| -32603     | Internal error - node processing issues               |
| -32000     | Server error - RPC endpoint unavailable               |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const balance = await provider.getBalance('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(ethers.formatEther(balance)); // Convert wei to SOMI
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.getBalance({
  address: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45'
});
console.log(formatEther(balance));
```
{% endcode %}
{% endtab %}
{% endtabs %}
