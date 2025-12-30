---
description: >-
  Example code for the eth_feeHistory JSON-RPC method. Complete guide on how to
  use eth_feeHistory JSON-RPC in GetBlock Web3 documentation.
---

# eth\_feeHistory - Base

The `eth_feeHistory` method returns historical gas information for EIP-1559 fee estimation. This method provides base fee history and reward percentiles to help calculate optimal transaction fees.

## Parameters

| Parameter         | Type   | Required | Description                                                  |
| ----------------- | ------ | -------- | ------------------------------------------------------------ |
| blockCount        | string | Yes      | Number of blocks in the requested range (hex, max 1024)      |
| newestBlock       | string | Yes      | Highest block of the range ("latest" or block number in hex) |
| rewardPercentiles | array  | No       | Array of percentile values (0-100) for priority fee sampling |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_feeHistory",
    "params": ["0xa", "latest", [25, 50, 75]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_feeHistory',
    params: ['0xa', 'latest', [25, 50, 75]],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const feeHistory = response.data.result;
console.log('Oldest Block:', parseInt(feeHistory.oldestBlock, 16));
console.log('Base Fees:', feeHistory.baseFeePerGas.map(f => parseInt(f, 16)));
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
        'method': 'eth_feeHistory',
        'params': ['0xa', 'latest', [25, 50, 75]],
        'id': 'getblock.io'
    }
)

result = response.json()
fee_history = result['result']
print(f'Oldest Block: {int(fee_history["oldestBlock"], 16)}')
print(f'Base Fees: {[int(f, 16) for f in fee_history["baseFeePerGas"]]}')
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
            "method": "eth_feeHistory",
            "params": ["0xa", "latest", [25, 50, 75]],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Fee History: {}", serde_json::to_string_pretty(&response["result"])?);
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
        "oldestBlock": "0x12d6876",
        "baseFeePerGas": [
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100",
            "0x5f5e100"
        ],
        "gasUsedRatio": [
            0.0,
            0.1234,
            0.0567,
            0.0,
            0.2345,
            0.0,
            0.0,
            0.0891,
            0.0,
            0.0
        ],
        "reward": [
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"],
            ["0x0", "0x0", "0x0"]
        ]
    }
}
```

## Response Parameters

| Parameter     | Type   | Description                                            |
| ------------- | ------ | ------------------------------------------------------ |
| oldestBlock   | string | Oldest block number in the returned range (hex)        |
| baseFeePerGas | array  | Array of base fees per gas (N+1 elements for N blocks) |
| gasUsedRatio  | array  | Array of gas used ratios (0 to 1) for each block       |
| reward        | array  | Array of priority fee arrays at requested percentiles  |

## Use Cases

1. **Fee Estimation:** Calculate optimal gas fees for transactions.
2. **Gas Price Prediction:** Forecast future base fees.
3. **Network Analysis:** analyze gas usage patterns.
4. **UI Display:** Show fee trends to users.
5. **MEV Strategies:** Optimize transaction priority fees.

## Error Handling

| Error Code | Message        | Description                        |
| ---------- | -------------- | ---------------------------------- |
| -32602     | Invalid params | Invalid block count or percentiles |
| -32603     | Internal error | Node internal failure              |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getFeeHistory() {
    // Get fee history for last 10 blocks
    const feeHistory = await provider.send('eth_feeHistory', ['0xa', 'latest', [25, 50, 75]]);
    
    console.log('Oldest Block:', parseInt(feeHistory.oldestBlock, 16));
    console.log('Base Fees:', feeHistory.baseFeePerGas.map(f => 
        ethers.formatUnits(f, 'gwei') + ' gwei'
    ));
    
    // Calculate suggested fees
    const latestBaseFee = BigInt(feeHistory.baseFeePerGas[feeHistory.baseFeePerGas.length - 1]);
    const suggestedMaxFee = latestBaseFee * 2n;
    
    console.log('Suggested Max Fee:', ethers.formatUnits(suggestedMaxFee, 'gwei'), 'gwei');
    
    return feeHistory;
}

// Using getFeeData (simpler)
async function getSimpleFeeData() {
    const feeData = await provider.getFeeData();
    console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'gwei');
    console.log('Max Fee:', ethers.formatUnits(feeData.maxFeePerGas, 'gwei'), 'gwei');
    console.log('Priority Fee:', ethers.formatUnits(feeData.maxPriorityFeePerGas, 'gwei'), 'gwei');
    return feeData;
}

getFeeHistory();
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

async function getFeeHistory() {
    const feeHistory = await client.getFeeHistory({
        blockCount: 10,
        rewardPercentiles: [25, 50, 75]
    });
    
    console.log('Base Fees:', feeHistory.baseFeePerGas.map(f => 
        formatGwei(f) + ' gwei'
    ));
    console.log('Gas Used Ratios:', feeHistory.gasUsedRatio);
    
    return feeHistory;
}

// Estimate fees for transaction
async function estimateFees() {
    const { maxFeePerGas, maxPriorityFeePerGas } = await client.estimateFeesPerGas();
    
    console.log('Max Fee:', formatGwei(maxFeePerGas), 'gwei');
    console.log('Priority Fee:', formatGwei(maxPriorityFeePerGas), 'gwei');
    
    return { maxFeePerGas, maxPriorityFeePerGas };
}

getFeeHistory();
```
{% endtab %}
{% endtabs %}
