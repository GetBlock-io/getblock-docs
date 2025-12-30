---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON-RPC method.
  Complete guide on how to use eth_getTransactionByBlockHashAndIndex JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Base

The `eth_getTransactionByBlockHashAndIndex` method returns information about a transaction by block hash and transaction index position. This is useful when you know the specific position of a transaction within a block.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| blockHash | string | Yes      | 32-byte hash of the block         |
| index     | string | Yes      | Transaction index position in hex |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const blockHash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: [blockHash, '0x0'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const tx = response.data.result;
if (tx) {
    console.log('From:', tx.from);
    console.log('To:', tx.to);
    console.log('Hash:', tx.hash);
}
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

block_hash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionByBlockHashAndIndex',
        'params': [block_hash, '0x0'],
        'id': 'getblock.io'
    }
)

result = response.json()
tx = result['result']
if tx:
    print(f'From: {tx["from"]}')
    print(f'To: {tx["to"]}')
    print(f'Hash: {tx["hash"]}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let block_hash = "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getTransactionByBlockHashAndIndex",
            "params": [block_hash, "0x0"],
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
        "hash": "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249",
        "input": "0x",
        "nonce": "0x1a",
        "to": "0x1234567890123456789012345678901234567890",
        "transactionIndex": "0x0",
        "value": "0xde0b6b3a7640000",
        "type": "0x2",
        "chainId": "0x2105"
    }
}
```

## Response Parameters

| Parameter        | Type   | Description                    |
| ---------------- | ------ | ------------------------------ |
| hash             | string | 32-byte transaction hash       |
| blockHash        | string | 32-byte block hash             |
| blockNumber      | string | Block number in hex            |
| from             | string | 20-byte sender address         |
| to               | string | 20-byte recipient address      |
| value            | string | Value transferred in wei (hex) |
| gas              | string | Gas limit provided (hex)       |
| transactionIndex | string | Index in block (hex)           |
| type             | string | Transaction type               |

## Use Cases

* Block Processing: Iterate through transactions by index
* First Transaction: Get the first transaction (often system tx on L2)
* MEV Analysis: Analyze transaction ordering within blocks
* Batch Processing: Process transactions sequentially\\
* Data Validation: Verify transaction position in block

## Error Handling

| Error Code | Message        | Description                        |
| ---------- | -------------- | ---------------------------------- |
| -32602     | Invalid params | Invalid block hash or index format |
| -32603     | Internal error | Block or transaction not found     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getTxByBlockHashAndIndex(blockHash, index) {
    // Get the block with transactions
    const block = await provider.getBlock(blockHash, true);
    
    if (block && block.transactions[index]) {
        const tx = block.transactions[index];
        console.log('Transaction Hash:', tx.hash);
        console.log('From:', tx.from);
        console.log('To:', tx.to);
        console.log('Value:', ethers.formatEther(tx.value), 'ETH');
        return tx;
    } else {
        console.log('Transaction not found');
        return null;
    }
}

getTxByBlockHashAndIndex(
    '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2',
    0
);
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

async function getTxByBlockHashAndIndex(blockHash, index) {
    const tx = await client.getTransaction({
        blockHash: blockHash,
        index: index
    });
    
    if (tx) {
        console.log('Transaction Hash:', tx.hash);
        console.log('From:', tx.from);
        console.log('To:', tx.to);
        console.log('Value:', formatEther(tx.value), 'ETH');
    }
    
    return tx;
}

getTxByBlockHashAndIndex(
    '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2',
    0
);
```
{% endtab %}
{% endtabs %}
