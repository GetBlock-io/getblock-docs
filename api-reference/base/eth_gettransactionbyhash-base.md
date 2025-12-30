---
description: >-
  Example code for the eth_getTransactionByHash JSON-RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Base

The `eth_getTransactionByHash` method returns the information about a transaction by its hash. This is the primary method for looking up transaction details, status, and metadata on the Base network.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const txHash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByHash',
    params: [txHash],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const tx = response.data.result;
if (tx) {
    console.log('From:', tx.from);
    console.log('To:', tx.to);
    console.log('Value:', parseInt(tx.value, 16) / 1e18, 'ETH');
    console.log('Block:', parseInt(tx.blockNumber, 16));
}
```
{% endtab %}

{% tab title="Requests (Python)" %}
```python
import requests

tx_hash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionByHash',
        'params': [tx_hash],
        'id': 'getblock.io'
    }
)

result = response.json()
tx = result['result']
if tx:
    print(f'From: {tx["from"]}')
    print(f'To: {tx["to"]}')
    print(f'Value: {int(tx["value"], 16) / 1e18} ETH')
    print(f'Block: {int(tx["blockNumber"], 16)}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let tx_hash = "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getTransactionByHash",
            "params": [tx_hash],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Transaction: {}", serde_json::to_string_pretty(&response["result"])?);
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
    "result": {
        "blockHash": "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2",
        "blockNumber": "0x12d687f",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "gas": "0x5208",
        "gasPrice": "0x5f5e100",
        "maxFeePerGas": "0x77359400",
        "maxPriorityFeePerGas": "0x3b9aca00",
        "hash": "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249",
        "input": "0x",
        "nonce": "0x1a",
        "to": "0x1234567890123456789012345678901234567890",
        "transactionIndex": "0x0",
        "value": "0xde0b6b3a7640000",
        "type": "0x2",
        "chainId": "0x2105",
        "v": "0x0",
        "r": "0x...",
        "s": "0x..."
    }
}
```

## Response Parameters

| Parameter            | Type   | Description                                            |
| -------------------- | ------ | ------------------------------------------------------ |
| hash                 | string | 32-byte transaction hash                               |
| blockHash            | string | 32-byte block hash (null if pending)                   |
| blockNumber          | string | Block number in hex (null if pending)                  |
| from                 | string | 20-byte sender address                                 |
| to                   | string | 20-byte recipient address (null for contract creation) |
| value                | string | Value transferred in wei (hex)                         |
| gas                  | string | Gas limit provided (hex)                               |
| gasPrice             | string | Gas price in wei (hex)                                 |
| maxFeePerGas         | string | EIP-1559 max fee per gas (hex)                         |
| maxPriorityFeePerGas | string | EIP-1559 max priority fee (hex)                        |
| input                | string | Transaction input data (hex)                           |
| nonce                | string | Sender's nonce (hex)                                   |
| transactionIndex     | string | Index in block (hex)                                   |
| type                 | string | Transaction type (0x2 for EIP-1559)                    |
| chainId              | string | Chain ID (0x2105 for Base mainnet)                     |

## Use Cases

* Transaction Tracking: Monitor transaction status and details
* Payment Verification: Confirm payment transactions
* Wallet History: Display transaction details to users
* Debugging: Analyze failed or stuck transactions
* Analytics: Collect transaction data for analysis

## Error Handling

| Error Code  | Message        | Description                          |
| ----------- | -------------- | ------------------------------------ |
| -32602      | Invalid params | Invalid transaction hash format      |
| -32603      | Internal error | Node internal failure                |
| null result | -              | Transaction not found (returns null) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getTransaction(txHash) {
    const tx = await provider.getTransaction(txHash);
    
    if (tx) {
        console.log('Hash:', tx.hash);
        console.log('From:', tx.from);
        console.log('To:', tx.to);
        console.log('Value:', ethers.formatEther(tx.value), 'ETH');
        console.log('Nonce:', tx.nonce);
        console.log('Gas Limit:', tx.gasLimit.toString());
        console.log('Block Number:', tx.blockNumber);
        
        // Check if confirmed
        if (tx.blockNumber) {
            const confirmations = await tx.confirmations();
            console.log('Confirmations:', confirmations);
        }
    } else {
        console.log('Transaction not found');
    }
    
    return tx;
}

getTransaction('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getTransaction(txHash) {
    const tx = await client.getTransaction({
        hash: txHash
    });
    
    console.log('Hash:', tx.hash);
    console.log('From:', tx.from);
    console.log('To:', tx.to);
    console.log('Value:', formatEther(tx.value), 'ETH');
    console.log('Nonce:', tx.nonce);
    console.log('Block Number:', tx.blockNumber);
    
    return tx;
}

getTransaction('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');
```
{% endtab %}
{% endtabs %}
