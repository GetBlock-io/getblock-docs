---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Base

The `eth_getBlockByHash` method returns information about a block by its hash. This method is useful when you have a specific block hash and need to retrieve the complete block data.

## Parameters

| Parameter        | Type    | Required | Description                                                                          |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------ |
| blockHash        | string  | Yes      | 32-byte hash of the block                                                            |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns only transaction hashes |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2", true],
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
    method: 'eth_getBlockByHash',
    params: [blockHash, true],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const block = response.data.result;
console.log('Block Number:', parseInt(block.number, 16));
console.log('Transactions:', block.transactions.length);
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
        'method': 'eth_getBlockByHash',
        'params': [block_hash, True],
        'id': 'getblock.io'
    }
)

result = response.json()
block = result['result']
if block:
    print(f'Block Number: {int(block["number"], 16)}')
    print(f'Transactions: {len(block["transactions"])}')
```
{% endtab %}

{% tab title="rust" %}
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
            "method": "eth_getBlockByHash",
            "params": [block_hash, true],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Block: {}", serde_json::to_string_pretty(&response["result"])?);
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
        "number": "0x12d687f",
        "hash": "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2",
        "parentHash": "0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890",
        "timestamp": "0x64a1b2c3",
        "transactions": [],
        "gasUsed": "0x5208",
        "gasLimit": "0x1c9c380",
        "baseFeePerGas": "0x5f5e100",
        "miner": "0x4200000000000000000000000000000000000011"
    }
}
```

## Response Parameters

| Parameter     | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| number        | string | Block number in hex                    |
| hash          | string | 32-byte block hash                     |
| parentHash    | string | 32-byte parent block hash              |
| timestamp     | string | Unix timestamp of block creation       |
| transactions  | array  | Array of transaction hashes or objects |
| gasUsed       | string | Total gas used by all transactions     |
| gasLimit      | string | Maximum gas allowed in block           |
| baseFeePerGas | string | EIP-1559 base fee per gas              |
| miner         | string | Address of the sequencer               |

## Use Cases

1. Block Verification: Verify block data using a known hash
2. Transaction Lookup: Retrieve all transactions from a specific block
3. Chain Analysis: Analyze block data for research purposes
4. Event Processing: Process events from a specific block by hash
5. Fork Detection: Compare blocks across different nodes

## Error Handling

| Error Code | Message         | Description                     |
| ---------- | --------------- | ------------------------------- |
| -32602     | Invalid params  | Invalid block hash format       |
| -32603     | Internal error  | Block not found or node failure |
| -32600     | Invalid request | Malformed JSON-RPC request      |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getBlockByHash(blockHash) {
    const block = await provider.getBlock(blockHash);
    
    if (block) {
        console.log('Block Number:', block.number);
        console.log('Hash:', block.hash);
        console.log('Timestamp:', new Date(block.timestamp * 1000));
        console.log('Transactions:', block.transactions.length);
    } else {
        console.log('Block not found');
    }
    
    return block;
}

getBlockByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
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

async function getBlockByHash(blockHash) {
    const block = await client.getBlock({
        blockHash: blockHash,
        includeTransactions: true
    });
    
    console.log('Block Number:', block.number);
    console.log('Hash:', block.hash);
    console.log('Transactions:', block.transactions.length);
    
    return block;
}

getBlockByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
```
{% endtab %}
{% endtabs %}
