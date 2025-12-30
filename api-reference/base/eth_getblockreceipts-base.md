---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Base

The `eth_getBlockReceipts` method returns all transaction receipts for a given block. This is more efficient than fetching individual receipts when you need all receipts from a block.

## Parameters

| Parameter      | Type   | Required | Description                                                         |
| -------------- | ------ | -------- | ------------------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, block hash, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockReceipts',
    params: ['latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const receipts = response.data.result;
console.log('Total Receipts:', receipts.length);
receipts.forEach(r => {
    console.log('Tx:', r.transactionHash, 'Status:', r.status === '0x1' ? 'Success' : 'Failed');
});
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Requests (Python)" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockReceipts',
        'params': ['latest'],
        'id': 'getblock.io'
    }
)

result = response.json()
receipts = result['result']
print(f'Total Receipts: {len(receipts)}')
for r in receipts:
    status = 'Success' if r['status'] == '0x1' else 'Failed'
    print(f'Tx: {r["transactionHash"]} Status: {status}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
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
            "method": "eth_getBlockReceipts",
            "params": ["latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    let receipts = response["result"].as_array().unwrap();
    println!("Total Receipts: {}", receipts.len());
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="Response (example)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "transactionHash": "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249",
            "transactionIndex": "0x0",
            "blockHash": "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2",
            "blockNumber": "0x12d687f",
            "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
            "to": "0x1234567890123456789012345678901234567890",
            "cumulativeGasUsed": "0x5208",
            "gasUsed": "0x5208",
            "effectiveGasPrice": "0x5f5e100",
            "contractAddress": null,
            "logs": [],
            "logsBloom": "0x00000000...",
            "status": "0x1",
            "type": "0x2",
            "l1Fee": "0x2540be400",
            "l1GasPrice": "0x3b9aca00",
            "l1GasUsed": "0x640"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type   | Description                        |
| ----------------- | ------ | ---------------------------------- |
| transactionHash   | string | 32-byte transaction hash           |
| blockHash         | string | 32-byte block hash                 |
| blockNumber       | string | Block number in hex                |
| from              | string | 20-byte sender address             |
| to                | string | 20-byte recipient address          |
| status            | string | 0x1 for success, 0x0 for failure   |
| gasUsed           | string | Gas used by this transaction (hex) |
| effectiveGasPrice | string | Actual gas price paid (hex)        |
| logs              | array  | Array of log objects               |
| l1Fee             | string | L1 data fee (Base-specific)        |

## Use Cases

1. Block Processing: Process all transactions in a block efficiently.
2. Event Indexing: Extract all events from a block at once
3. Analytics: Analyze gas usage across entire blocks.
4. Chain Indexing: Build block-by-block indexes.
5. MEV Analysis: Examine transaction ordering and outcomes.

## Error Handling

| Error Code | Message        | Description                     |
| ---------- | -------------- | ------------------------------- |
| -32602     | Invalid params | Invalid block parameter         |
| -32603     | Internal error | Block not found or node failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getBlockReceipts(blockNumber = 'latest') {
    const receipts = await provider.send('eth_getBlockReceipts', [blockNumber]);
    
    console.log('Total Receipts:', receipts.length);
    
    let totalGasUsed = 0n;
    receipts.forEach(receipt => {
        totalGasUsed += BigInt(receipt.gasUsed);
        console.log('Tx:', receipt.transactionHash);
        console.log('  Status:', receipt.status === '0x1' ? 'Success' : 'Failed');
        console.log('  Gas Used:', parseInt(receipt.gasUsed, 16));
    });
    
    console.log('Total Block Gas Used:', totalGasUsed.toString());
    return receipts;
}

getBlockReceipts();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getBlockReceipts(blockNumber = 'latest') {
    const receipts = await client.request({
        method: 'eth_getBlockReceipts',
        params: [blockNumber]
    });
    
    console.log('Total Receipts:', receipts.length);
    
    receipts.forEach(receipt => {
        console.log('Tx:', receipt.transactionHash);
        console.log('  Status:', receipt.status);
        console.log('  Logs:', receipt.logs.length);
    });
    
    return receipts;
}

getBlockReceipts();
```
{% endcode %}
{% endtab %}
{% endtabs %}
