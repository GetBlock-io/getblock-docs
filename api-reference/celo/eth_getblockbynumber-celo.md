---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - Celo

The  method returns information about a block by block number on the Celo network. With Celo's \~1 second block times and one-block finality, blocks are produced rapidly and confirmed instantly.

## Parameters

| Parameter        | Type    | Required | Description                                                                     |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------- |
| blockNumber      | string  | Yes      | Block number in hex, or "latest", "earliest", "pending"                         |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns transaction hashes |

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
  "method": "eth_getBlockByNumber",
  "params": ["latest", false]
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
  method: 'eth_getBlockByNumber',
  params: ['latest', false]
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
    "method": "eth_getBlockByNumber",
    "params": ["latest", False]
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
        "method": "eth_getBlockByNumber",
        "params": ["latest", false]
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
  "result": {
    "number": "0x1d9f2a8",
    "hash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    "parentHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
    "timestamp": "0x65a1b2c3",
    "gasLimit": "0x1c9c380",
    "gasUsed": "0x5208",
    "miner": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "transactions": [
      "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
    ]
  }
}
```

## Response Parameters

| Parameter    | Type   | Description               |
| ------------ | ------ | ------------------------- |
| number       | string | Block number in hex       |
| hash         | string | 32-byte block hash        |
| parentHash   | string | 32-byte parent block hash |
| timestamp    | string | Unix timestamp            |
| gasLimit     | string | Maximum gas for block     |
| gasUsed      | string | Total gas consumed        |
| miner        | string | Block producer address    |
| transactions | array  | Transaction data          |

## Use Cases

* Fetch block data for explorers
* Monitor block production
* Analyze transaction throughput
* Track gas usage patterns
* Build indexing services

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - invalid block number   |
| -32603      | Internal error - node processing issues |
| null result | Block not found                         |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('latest');
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { celo } from 'viem/chains';

const client = createPublicClient({
  chain: celo,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const block = await client.getBlock({ blockTag: 'latest' });
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await kit.web3.eth.getBlock('latest');
console.log(block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
