---
description: >-
  Example code for the eth_getTransactionByHash JSON-RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Somnia

This method returns information about a transaction by its hash on the Somnia network. This is essential for tracking transaction status, debugging, and building transaction explorers. Somnia's sub-second finality means transactions are confirmed rapidly.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Returns

| Field       | Type   | Description                    |
| ----------- | ------ | ------------------------------ |
| hash        | string | Transaction hash               |
| from        | string | Sender address                 |
| to          | string | Recipient address              |
| value       | string | Value transferred in wei       |
| gas         | string | Gas provided                   |
| gasPrice    | string | Gas price in wei               |
| input       | string | Transaction input data         |
| nonce       | string | Sender's nonce                 |
| blockHash   | string | Block hash (null if pending)   |
| blockNumber | string | Block number (null if pending) |

## Request Example

### cURL

{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getTransactionByHash",
  "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]
}'
```
{% endcode %}

### JavaScript (Axios)

{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getTransactionByHash',
  params: ['0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}

### Python

{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}

### Rust

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
        "method": "eth_getTransactionByHash",
        "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"]
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

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "nonce": "0x15",
    "blockHash": "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    "blockNumber": "0x1a2b3c",
    "transactionIndex": "0x0",
    "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
    "value": "0xde0b6b3a7640000",
    "gas": "0x5208",
    "gasPrice": "0x3b9aca00",
    "input": "0x"
  }
}
```
{% endcode %}

## Response Parameters

| Parameter   | Type   | Description                                    |
| ----------- | ------ | ---------------------------------------------- |
| hash        | string | 32-byte transaction hash                       |
| from        | string | Sender address                                 |
| to          | string | Recipient address (null for contract creation) |
| value       | string | Value in wei                                   |
| gas         | string | Gas limit                                      |
| gasPrice    | string | Gas price in wei                               |
| input       | string | Call data                                      |
| nonce       | string | Transaction nonce                              |
| blockHash   | string | Containing block hash                          |
| blockNumber | string | Containing block number                        |

## Use Cases

* Track transaction status
* Verify transaction details
* Build transaction explorers
* Debug failed transactions
* Monitor pending transactions

## Error Handling

| Error Code  | Description                             |
| ----------- | --------------------------------------- |
| -32602      | Invalid params - malformed hash         |
| -32603      | Internal error - node processing issues |
| null result | Transaction not found                   |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.getTransaction('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
console.log(tx);
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

const tx = await client.getTransaction({
  hash: '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b'
});
console.log(tx);
```
{% endcode %}
{% endtab %}
{% endtabs %}
