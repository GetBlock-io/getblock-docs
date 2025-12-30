---
description: >-
  Example code for the eth_gasPrice JSON-RPC method. Complete guide on how to
  use eth_gasPrice JSON-RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - Base

The `eth_gasPrice` method returns the current gas price on the Base network in wei. This value represents the L2 execution fee component. Note that Base transactions also incur an L1 data fee for posting transaction data to Ethereum mainnet.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_gasPrice',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const gasPriceWei = parseInt(response.data.result, 16);
const gasPriceGwei = gasPriceWei / 1e9;
console.log('Gas Price:', gasPriceGwei, 'gwei');
```
{% endtab %}

{% tab title="Requests (Python)" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_gasPrice',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
gas_price_wei = int(result['result'], 16)
gas_price_gwei = gas_price_wei / 1e9
print(f'Gas Price: {gas_price_gwei} gwei')
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "eth_gasPrice",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Gas Price: {}", response["result"]);
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
    "result": "0x5f5e100"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Hexadecimal gas price in wei            |

## Use Cases

* Transaction Fee Estimation: Calculate L2 execution costs before sending transactions
* Gas Price Monitoring: Track network congestion and fee trends
* Cost Optimization: Time transactions for lower fees during quiet periods
* User Interface Display: Show current gas prices to users
* Trading Bot Configuration: Set gas price limits for automated trading

## Error Handling

| Error Code | Message         | Description                |
| ---------- | --------------- | -------------------------- |
| -32603     | Internal error  | Node internal failure      |
| -32600     | Invalid request | Malformed JSON-RPC request |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getGasPrice() {
    const feeData = await provider.getFeeData();
    
    console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'gwei');
    console.log('Max Fee Per Gas:', ethers.formatUnits(feeData.maxFeePerGas, 'gwei'), 'gwei');
    console.log('Max Priority Fee:', ethers.formatUnits(feeData.maxPriorityFeePerGas, 'gwei'), 'gwei');
    
    return feeData;
}

getGasPrice();
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

async function getGasPrice() {
    const gasPrice = await client.getGasPrice();
    console.log('Gas Price:', formatGwei(gasPrice), 'gwei');
    return gasPrice;
}

getGasPrice();
```
{% endtab %}
{% endtabs %}
