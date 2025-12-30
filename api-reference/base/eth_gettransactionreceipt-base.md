---
description: >-
  Example code for the eth_getTransactionByHash JSON-RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - Base

The `eth_getTransactionReceipt` method returns the receipt of a transaction by transaction hash. The receipt contains information about the transaction execution, including status, gas used, logs, and contract address for deployments.

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
    "method": "eth_getTransactionReceipt",
    "params": ["0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const txHash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionReceipt',
    params: [txHash],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const receipt = response.data.result;
if (receipt) {
    console.log('Status:', receipt.status === '0x1' ? 'Success' : 'Failed');
    console.log('Gas Used:', parseInt(receipt.gasUsed, 16));
    console.log('Logs:', receipt.logs.length);
}
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

tx_hash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionReceipt',
        'params': [tx_hash],
        'id': 'getblock.io'
    }
)

result = response.json()
receipt = result['result']
if receipt:
    status = 'Success' if receipt['status'] == '0x1' else 'Failed'
    print(f'Status: {status}')
    print(f'Gas Used: {int(receipt["gasUsed"], 16)}')
    print(f'Logs: {len(receipt["logs"])}')
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
            "method": "eth_getTransactionReceipt",
            "params": [tx_hash],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Receipt: {}", serde_json::to_string_pretty(&response["result"])?);
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
}
```

## Response Parameters

| Parameter         | Type   | Description                                    |
| ----------------- | ------ | ---------------------------------------------- |
| transactionHash   | string | 32-byte transaction hash                       |
| blockHash         | string | 32-byte block hash                             |
| blockNumber       | string | Block number in hex                            |
| from              | string | 20-byte sender address                         |
| to                | string | 20-byte recipient address                      |
| status            | string | 0x1 for success, 0x0 for failure               |
| gasUsed           | string | Gas used by this transaction (hex)             |
| effectiveGasPrice | string | Actual gas price paid (hex)                    |
| contractAddress   | string | Contract address if deployment, null otherwise |
| logs              | array  | Array of log objects                           |
| l1Fee             | string | L1 data fee component (Base-specific)          |
| l1GasPrice        | string | L1 gas price used for fee calculation          |
| l1GasUsed         | string | L1 gas used for data posting                   |

## Use Cases

* Transaction Confirmation: Verify transaction success or failure.
* Event Processing: Extract emitted events from logs.
* Contract Deployment: Get deployed contract address.
* Gas Analysis: Analyze actual gas consumption.
* Fee Calculation: Calculate total transaction cost including L1 fees.

## Error Handling

| Error Code  | Message        | Description                      |
| ----------- | -------------- | -------------------------------- |
| -32602      | Invalid params | Invalid transaction hash format  |
| -32603      | Internal error | Node internal failure            |
| null result | -              | Transaction pending or not found |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getReceipt(txHash) {
    const receipt = await provider.getTransactionReceipt(txHash);
    
    if (receipt) {
        console.log('Status:', receipt.status === 1 ? 'Success' : 'Failed');
        console.log('Block Number:', receipt.blockNumber);
        console.log('Gas Used:', receipt.gasUsed.toString());
        console.log('Effective Gas Price:', ethers.formatUnits(receipt.gasPrice, 'gwei'), 'gwei');
        console.log('Logs:', receipt.logs.length);
        
        // If contract deployment
        if (receipt.contractAddress) {
            console.log('Contract Address:', receipt.contractAddress);
        }
        
        // Parse logs
        receipt.logs.forEach((log, index) => {
            console.log(`Log ${index}:`, log.topics[0]);
        });
    } else {
        console.log('Receipt not found (transaction may be pending)');
    }
    
    return receipt;
}

getReceipt('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getReceipt(txHash) {
    const receipt = await client.getTransactionReceipt({
        hash: txHash
    });
    
    console.log('Status:', receipt.status);
    console.log('Block Number:', receipt.blockNumber);
    console.log('Gas Used:', receipt.gasUsed.toString());
    console.log('Effective Gas Price:', formatGwei(receipt.effectiveGasPrice), 'gwei');
    console.log('Logs:', receipt.logs.length);
    
    // If contract deployment
    if (receipt.contractAddress) {
        console.log('Contract Address:', receipt.contractAddress);
    }
    
    return receipt;
}

getReceipt('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');

// Wait for transaction receipt
async function waitForReceipt(txHash) {
    const receipt = await client.waitForTransactionReceipt({
        hash: txHash
    });
    return receipt;
}
```
{% endtab %}
{% endtabs %}
