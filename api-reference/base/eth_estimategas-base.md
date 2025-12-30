---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Base

The `eth_estimateGas` method generates and returns an estimate of how much gas is necessary to allow a transaction to complete. This estimate may be significantly more than the actual gas used due to EVM mechanics and node performance variations.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                 |
| blockParameter | string | No       | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description                                                |
| -------- | ------ | -------- | ---------------------------------------------------------- |
| from     | string | No       | 20-byte sender address                                     |
| to       | string | No       | 20-byte recipient address (optional for contract creation) |
| gas      | string | No       | Gas limit (hex)                                            |
| gasPrice | string | No       | Gas price in wei (hex)                                     |
| value    | string | No       | Value to send in wei (hex)                                 |
| data     | string | No       | Transaction data (hex)                                     |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0x4200000000000000000000000000000000000006",
        "data": "0xa9059cbb0000000000000000000000001234567890123456789012345678901234567890000000000000000000000000000000000000000000000000016345785d8a0000"
    }],
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
    method: 'eth_estimateGas',
    params: [{
        from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21',
        to: '0x4200000000000000000000000000000000000006',
        data: '0xa9059cbb0000000000000000000000001234567890123456789012345678901234567890000000000000000000000000000000000000000000000000016345785d8a0000'
    }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const gasEstimate = parseInt(response.data.result, 16);
console.log('Gas Estimate:', gasEstimate);
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
        'method': 'eth_estimateGas',
        'params': [{
            'from': '0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21',
            'to': '0x4200000000000000000000000000000000000006',
            'data': '0xa9059cbb0000000000000000000000001234567890123456789012345678901234567890000000000000000000000000000000000000000000000000016345785d8a0000'
        }],
        'id': 'getblock.io'
    }
)

result = response.json()
gas_estimate = int(result['result'], 16)
print(f'Gas Estimate: {gas_estimate}')
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
            "method": "eth_estimateGas",
            "params": [{
                "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
                "to": "0x4200000000000000000000000000000000000006",
                "data": "0xa9059cbb..."
            }],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Gas Estimate: {}", response["result"]);
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
    "result": "0xc350"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Estimated gas required in hex           |

## Use Cases

* Gas Limit Setting: Determine appropriate gas limit for transactions
* Cost Estimation: Calculate transaction costs before sending
* UI Display: Show estimated fees to users before confirmation
* Transaction Validation: Pre-validate transactions will succeed
* Batch Processing: Estimate gas for multiple operations

## Error Handling

| Error Code | Message            | Description                           |
| ---------- | ------------------ | ------------------------------------- |
| -32602     | Invalid params     | Invalid transaction object            |
| -32603     | Internal error     | Estimation failed                     |
| 3          | Execution reverted | Transaction would revert              |
| -32000     | Execution error    | Insufficient balance or other failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function estimateGas(tx) {
    try {
        const gasEstimate = await provider.estimateGas(tx);
        console.log('Gas Estimate:', gasEstimate.toString());
        
        // Add 20% buffer for safety
        const gasLimit = gasEstimate * 120n / 100n;
        console.log('Gas Limit (with buffer):', gasLimit.toString());
        
        return gasLimit;
    } catch (error) {
        console.error('Estimation failed:', error.message);
        throw error;
    }
}

// ETH transfer
estimateGas({
    from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21',
    to: '0x1234567890123456789012345678901234567890',
    value: ethers.parseEther('0.1')
});

// Contract call
async function estimateContractGas(contractAddress, abi, functionName, args, from) {
    const contract = new ethers.Contract(contractAddress, abi, provider);
    
    const gasEstimate = await contract[functionName].estimateGas(...args, { from });
    console.log('Contract Gas Estimate:', gasEstimate.toString());
    
    return gasEstimate;
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function estimateGas(tx) {
    try {
        const gasEstimate = await client.estimateGas({
            account: tx.from,
            to: tx.to,
            value: tx.value,
            data: tx.data
        });
        
        console.log('Gas Estimate:', gasEstimate.toString());
        
        // Add 20% buffer
        const gasLimit = gasEstimate * 120n / 100n;
        console.log('Gas Limit (with buffer):', gasLimit.toString());
        
        return gasLimit;
    } catch (error) {
        console.error('Estimation failed:', error.message);
        throw error;
    }
}

// ETH transfer
estimateGas({
    from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21',
    to: '0x1234567890123456789012345678901234567890',
    value: parseEther('0.1')
});

// Contract call estimation
async function estimateContractGas(contractAddress, abi, functionName, args, account) {
    const gasEstimate = await client.estimateContractGas({
        address: contractAddress,
        abi: abi,
        functionName: functionName,
        args: args,
        account: account
    });
    
    console.log('Contract Gas Estimate:', gasEstimate.toString());
    return gasEstimate;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
