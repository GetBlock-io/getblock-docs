---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON-RPC method. Complete guide
  on how to use eth_maxPriorityFeePerGas JSON-RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Base

The `eth_maxPriorityFeePerGas` method returns a suggested max priority fee per gas (tip) for EIP-1559 transactions. On Base, priority fees are typically low since the sequencer processes transactions in order of receipt.

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
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
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
    method: 'eth_maxPriorityFeePerGas',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const priorityFee = parseInt(response.data.result, 16);
console.log('Priority Fee:', priorityFee / 1e9, 'gwei');
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
        'method': 'eth_maxPriorityFeePerGas',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
priority_fee = int(result['result'], 16)
print(f'Priority Fee: {priority_fee / 1e9} gwei')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_maxPriorityFeePerGas",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Priority Fee: {}", response["result"]);
    Ok(())
}
```
{% endcode %}


{% endtab %}
{% endtabs %}

## Response

{% code title="Example Response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xf4240"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")               |
| id        | string | Request identifier matching the request         |
| result    | string | Suggested max priority fee per gas in wei (hex) |

## Use Cases

{% stepper %}
{% step %}
### Fee Estimation

Get suggested priority fee for transactions.
{% endstep %}

{% step %}
### EIP-1559 Transactions

Set appropriate priority fee.
{% endstep %}

{% step %}
### Cost Optimization

Balance speed vs cost.
{% endstep %}

{% step %}
### Gas Strategies

Implement dynamic fee strategies.
{% endstep %}
{% endstepper %}

## Error Handling

| Error Code | Message        | Description           |
| ---------- | -------------- | --------------------- |
| -32603     | Internal error | Node internal failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getFeeData() {
    const feeData = await provider.getFeeData();
    
    console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'gwei');
    console.log('Max Fee Per Gas:', ethers.formatUnits(feeData.maxFeePerGas, 'gwei'), 'gwei');
    console.log('Max Priority Fee:', ethers.formatUnits(feeData.maxPriorityFeePerGas, 'gwei'), 'gwei');
    
    return feeData;
}

getFeeData();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getMaxPriorityFee() {
    const { maxFeePerGas, maxPriorityFeePerGas } = await client.estimateFeesPerGas();
    
    console.log('Max Fee:', formatGwei(maxFeePerGas), 'gwei');
    console.log('Priority Fee:', formatGwei(maxPriorityFeePerGas), 'gwei');
    
    return { maxFeePerGas, maxPriorityFeePerGas };
}

getMaxPriorityFee();
```
{% endcode %}
{% endtab %}
{% endtabs %}
