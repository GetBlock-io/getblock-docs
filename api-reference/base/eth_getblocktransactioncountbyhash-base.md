---
description: >-
  Example code for the eth_getBlockTransactionCountByHash JSON-RPC method.
  Complete guide on how to use eth_getBlockTransactionCountByHash JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - Base

The `eth_getBlockTransactionCountByHash` method returns the number of transactions in a block matching the given block hash. This is useful for quickly determining block activity without fetching full block data.

## Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| blockHash | string | Yes      | 32-byte hash of the block |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const blockHash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByHash',
    params: [blockHash],
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
{% code title="example.py" %}
```python
import requests

block_hash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockTransactionCountByHash',
        'params': [block_hash],
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
    let block_hash = "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBlockTransactionCountByHash",
            "params": [block_hash],
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

{% hint style="info" %}
The JSON-RPC response returns the transaction count as a hex string (for example, "0x8"). Convert it to a decimal integer in your client (examples above use parseInt(..., 16) or int(..., 16)).
{% endhint %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x8"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | string | Number of transactions in the block (hex) |

## Use Cases

* Block Analysis: Quickly assess block activity levels
* Network Monitoring: Track transaction throughput
* Data Validation: Verify block data integrity
* Indexing Optimization: Plan batch processing based on tx count
* Statistics Collection: Gather block-level metrics

## Error Handling

| Error Code | Message        | Description               |
| ---------- | -------------- | ------------------------- |
| -32602     | Invalid params | Invalid block hash format |
| -32603     | Internal error | Block not found           |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getTxCountByHash(blockHash) {
    const block = await provider.getBlock(blockHash);
    
    if (block) {
        console.log('Block Number:', block.number);
        console.log('Transaction Count:', block.transactions.length);
        return block.transactions.length;
    } else {
        console.log('Block not found');
        return null;
    }
}

getTxCountByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
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

async function getTxCountByHash(blockHash) {
    const count = await client.getBlockTransactionCount({
        blockHash: blockHash
    });
    
    console.log('Transaction Count:', count);
    return count;
}

getTxCountByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
```
{% endcode %}
{% endtab %}
{% endtabs %}
