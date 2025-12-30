---
description: >-
  Example code for the eth_blockNumber JSON-RPC method. Complete guide on how to
  use eth_blockNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - Base

The `eth_blockNumber` method returns the number of the most recent block on the Base blockchain. This is a fundamental method for tracking chain progress and is commonly used for synchronization, monitoring, and determining confirmation counts.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
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
    method: 'eth_blockNumber',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Block Number:', parseInt(response.data.result, 16));
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
        'method': 'eth_blockNumber',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
block_number = int(result['result'], 16)
print(f'Block Number: {block_number}')
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
            "method": "eth_blockNumber",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Block Number: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x12d687f"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | string | Hexadecimal representation of the current block number |

## Use Cases

* Chain Synchronization: Monitor sync progress by comparing local block number with the network
* Transaction Confirmation: Calculate confirmation count by subtracting transaction block from current block
* Event Indexing: Determine starting point for event log queries
* Health Monitoring: Track block production rate to detect chain issues
* Block Polling: Implement block-based polling for new transactions or events

## Error Handling

| Error Code | Message         | Description                                    |
| ---------- | --------------- | ---------------------------------------------- |
| -32603     | Internal error  | Node synchronization issue or internal failure |
| -32600     | Invalid request | Malformed JSON-RPC request                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getBlockNumber() {
    const blockNumber = await provider.getBlockNumber();
    console.log('Current Block:', blockNumber);
    return blockNumber;
}

getBlockNumber();
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

async function getBlockNumber() {
    const blockNumber = await client.getBlockNumber();
    console.log('Current Block:', blockNumber);
    return blockNumber;
}

getBlockNumber();
```
{% endcode %}
{% endtab %}
{% endtabs %}
