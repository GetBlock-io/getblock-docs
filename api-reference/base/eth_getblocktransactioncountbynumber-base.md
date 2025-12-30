---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber JSON-RPC method.
  Complete guide on how to use eth_getBlockTransactionCountByNumber JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Base

The `eth_getBlockTransactionCountByNumber` method returns the number of transactions in a block matching the given block number. This provides a quick way to assess block activity without fetching full block data.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByNumber',
    params: ['latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const txCount = parseInt(response.data.result, 16);
console.log('Transaction Count:', txCount);
```
{% endcode %}
{% endtab %}

{% tab title="Requests (Python)" %}
{% code title="requests.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockTransactionCountByNumber',
        'params': ['latest'],
        'id': 'getblock.io'
    }
)

result = response.json()
tx_count = int(result['result'], 16)
print(f'Transaction Count: {tx_count}')
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
            "method": "eth_getBlockTransactionCountByNumber",
            "params": ["latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Transaction Count: {}", response["result"]);
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
    "result": "0xc"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | string | Number of transactions in the block (hex) |

## Use Cases

* Block Monitoring: Track transaction counts in recent blocks
* Network Activity: Measure current network throughput
* Historical Analysis: Analyze transaction patterns over time
* Resource Planning: Estimate processing requirements
* Dashboard Metrics: Display real-time block statistics

## Error Handling

| Error Code | Message        | Description                 |
| ---------- | -------------- | --------------------------- |
| -32602     | Invalid params | Invalid block number format |
| -32603     | Internal error | Block not found             |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getTxCountByNumber(blockNumber = 'latest') {
    const block = await provider.getBlock(blockNumber);
    
    if (block) {
        console.log('Block Number:', block.number);
        console.log('Transaction Count:', block.transactions.length);
        return block.transactions.length;
    } else {
        console.log('Block not found');
        return null;
    }
}

// Get latest block tx count
getTxCountByNumber();

// Get specific block tx count
getTxCountByNumber(19800000);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getTxCountByNumber(blockNumber) {
    const count = await client.getBlockTransactionCount({
        blockNumber: blockNumber ? BigInt(blockNumber) : undefined
    });
    
    console.log('Transaction Count:', count);
    return count;
}

// Get latest block tx count
getTxCountByNumber();

// Get specific block tx count
getTxCountByNumber(19800000);
```
{% endcode %}
{% endtab %}
{% endtabs %}
