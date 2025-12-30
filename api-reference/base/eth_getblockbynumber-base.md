---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - Base

The `eth_getBlockByNumber` method returns information about a block by block number. This method is essential for block explorers, indexers, and applications that need to process blockchain data sequentially.

## Parameters

| Parameter        | Type    | Required | Description                                                                          |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------ |
| blockNumber      | string  | Yes      | Block number in hex, or "latest", "earliest", "pending"                              |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns only transaction hashes |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockByNumber',
    params: ['latest', true],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const block = response.data.result;
console.log('Block Number:', parseInt(block.number, 16));
console.log('Transactions:', block.transactions.length);
```
{% endcode %}
{% endtab %}

{% tab title="Requests (Python)" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockByNumber',
        'params': ['latest', True],
        'id': 'getblock.io'
    }
)

result = response.json()
block = result['result']
print(f'Block Number: {int(block["number"], 16)}')
print(f'Timestamp: {int(block["timestamp"], 16)}')
print(f'Transactions: {len(block["transactions"])}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
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
            "method": "eth_getBlockByNumber",
            "params": ["latest", false],
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "number": "0x12d687f",
        "hash": "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
        "parentHash": "0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890",
        "nonce": "0x0000000000000000",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0x00000000000000000000000000000000...",
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "miner": "0x4200000000000000000000000000000000000011",
        "difficulty": "0x0",
        "totalDifficulty": "0x0",
        "extraData": "0x",
        "size": "0x220",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0x5208",
        "timestamp": "0x64a1b2c3",
        "transactions": [],
        "uncles": [],
        "baseFeePerGas": "0x5f5e100"
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
| size          | string | Size of the block in bytes             |

## Use Cases

1. Block Explorer: Display block details and transaction lists.
2. Chain Indexing: Process blocks sequentially for data indexing.
3. Transaction Analysis: Examine all transactions in a specific block.
4. Historical Data: Query blocks at specific heights for analysis.
5. Real-time Monitoring: Poll for new blocks and process transactions.

## Error Handling

| Error Code | Message         | Description                     |
| ---------- | --------------- | ------------------------------- |
| -32602     | Invalid params  | Invalid block number format     |
| -32603     | Internal error  | Block not found or node failure |
| -32600     | Invalid request | Malformed JSON-RPC request      |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getBlock(blockNumber = 'latest') {
    const block = await provider.getBlock(blockNumber);
    
    console.log('Block Number:', block.number);
    console.log('Hash:', block.hash);
    console.log('Timestamp:', new Date(block.timestamp * 1000));
    console.log('Transactions:', block.transactions.length);
    console.log('Gas Used:', block.gasUsed.toString());
    console.log('Base Fee:', ethers.formatUnits(block.baseFeePerGas, 'gwei'), 'gwei');
    
    return block;
}

getBlock();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getBlock(blockNumber = 'latest') {
    const block = await client.getBlock({
        blockNumber: blockNumber === 'latest' ? undefined : BigInt(blockNumber),
        includeTransactions: false
    });
    
    console.log('Block Number:', block.number);
    console.log('Hash:', block.hash);
    console.log('Timestamp:', new Date(Number(block.timestamp) * 1000));
    console.log('Transactions:', block.transactions.length);
    
    return block;
}

getBlock();
```
{% endcode %}
{% endtab %}
{% endtabs %}
