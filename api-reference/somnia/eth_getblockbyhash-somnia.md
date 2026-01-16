---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Somnia

This method returns information about a block by its hash on the Somnia network. This is useful for retrieving specific block data when you have the block hash, particularly for verifying block content and transaction inclusion.

## Parameters

| Parameter        | Type    | Required | Description                                                                     |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------- |
| blockHash        | string  | Yes      | 32-byte block hash                                                              |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns transaction hashes |

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
  "method": "eth_getBlockByHash",
  "params": [
    "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    false
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="axios.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getBlockByHash',
  params: ['0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd', false]
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getBlockByHash",
    "params": [
        "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        False
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
        "method": "eth_getBlockByHash",
        "params": [
            "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
            false
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
  "result": {
    "number": "0x1a2b3c",
    "hash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    "parentHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
    "timestamp": "0x65a1b2c3",
    "gasLimit": "0x1c9c380",
    "gasUsed": "0x5208",
    "miner": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "transactions": []
  }
}
```
{% endcode %}

## Response Parameters

| Parameter    | Type   | Description         |
| ------------ | ------ | ------------------- |
| number       | string | Block number in hex |
| hash         | string | 32-byte block hash  |
| parentHash   | string | 32-byte parent hash |
| timestamp    | string | Unix timestamp      |
| gasLimit     | string | Block gas limit     |
| gasUsed      | string | Total gas used      |
| transactions | array  | Transaction data    |

## Use Cases

* Verify block content by hash
* Fetch block for transaction verification
* Build block explorers
* Verify transaction inclusion proofs
* Historical data analysis

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - malformed hash         |
| -32603      | Internal error - node processing issues |
| null result | Block not found                         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd');
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const block = await client.getBlock({
  blockHash: '0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd'
});
console.log(block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
