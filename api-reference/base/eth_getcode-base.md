---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Base

The `eth_getCode` method returns the bytecode at a given address. This is used to determine if an address is a smart contract and to retrieve the deployed contract code.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to check                                |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x4200000000000000000000000000000000000006", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getCode',
    params: ['0x4200000000000000000000000000000000000006', 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const code = response.data.result;
const isContract = code !== '0x';
console.log('Is Contract:', isContract);
console.log('Code Length:', (code.length - 2) / 2, 'bytes');
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
        'method': 'eth_getCode',
        'params': ['0x4200000000000000000000000000000000000006', 'latest'],
        'id': 'getblock.io'
    }
)

result = response.json()
code = result['result']
is_contract = code != '0x'
print(f'Is Contract: {is_contract}')
print(f'Code Length: {(len(code) - 2) // 2} bytes')
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
            "method": "eth_getCode",
            "params": ["0x4200000000000000000000000000000000000006", "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    let code = response["result"].as_str().unwrap();
    let is_contract = code != "0x";
    println!("Is Contract: {}", is_contract);
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
    "result": "0x608060405234801561001057600080fd5b50..."
}
```

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                    |
| id        | string | Request identifier matching the request              |
| result    | string | Bytecode at the address (hex). Returns "0x" for EOAs |

## Use Cases

* Contract Detection: Verify if address is a smart contract
* Security Analysis: Examine contract bytecode for vulnerabilities
* Contract Verification: Compare deployed code with source
* Proxy Detection: Identify proxy patterns in bytecode
* Contract Indexing: Catalog deployed contracts

## Error Handling

| Error Code | Message        | Description            |
| ---------- | -------------- | ---------------------- |
| -32602     | Invalid params | Invalid address format |
| -32603     | Internal error | Node internal failure  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getCode(address) {
    const code = await provider.getCode(address);
    
    const isContract = code !== '0x';
    console.log('Address:', address);
    console.log('Is Contract:', isContract);
    
    if (isContract) {
        console.log('Code Length:', (code.length - 2) / 2, 'bytes');
        console.log('Code Preview:', code.slice(0, 66) + '...');
    }
    
    return code;
}

// Check WETH on Base
getCode('0x4200000000000000000000000000000000000006');

// Check EOA
getCode('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21');
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

async function getCode(address) {
    const code = await client.getCode({
        address: address
    });
    
    const isContract = code !== undefined && code !== '0x';
    console.log('Address:', address);
    console.log('Is Contract:', isContract);
    
    if (isContract) {
        console.log('Code Length:', (code.length - 2) / 2, 'bytes');
    }
    
    return code;
}

// Check WETH on Base
getCode('0x4200000000000000000000000000000000000006');
```
{% endtab %}
{% endtabs %}
