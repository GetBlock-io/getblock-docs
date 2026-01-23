---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide on how to
  use eth_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - Celo

This method returns the balance of the account at a given address on the Celo network. The balance is returned in wei (the smallest unit of CELO, where 1 CELO = 10^18 wei). This method is essential for wallet applications, balance verification, and transaction preparation.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | The address to check balance (20 bytes, 0x-prefixed)    |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
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
{% endtab %}

{% tab title="JavaScript (Axios)" %}
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
{% endtab %}

{% tab title="Python" %}
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
{% endtab %}

{% tab title="Rust" %}
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
{% endtab %}
{% endtabs %}

## Returns

| Field  | Type   | Description                                |
| ------ | ------ | ------------------------------------------ |
| result | string | The balance in wei as a hexadecimal string |

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x8ac7230489e80000"
}
```

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| result    | string | Balance in wei as hex (0x8ac7230489e80000 = 10 CELO) |

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
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const balance = await provider.getBalance('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(ethers.formatEther(balance)); // Convert wei to CELO
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { celo } from 'viem/chains';

const client = createPublicClient({
  chain: celo,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.getBalance({
  address: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45'
});
console.log(formatEther(balance));
```
{% endtab %}

{% tab title="ContractKit" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const balance = await kit.web3.eth.getBalance('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(kit.web3.utils.fromWei(balance, 'ether'));
```
{% endtab %}
{% endtabs %}
