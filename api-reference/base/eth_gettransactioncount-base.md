---
description: >-
  Example code for the eth_getTransactionCount  JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Base

The `eth_getTransactionCount` method returns the number of transactions sent from an address, also known as the nonce. This value is essential for constructing new transactions as each transaction must have a unique, sequential nonce.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to check                                |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionCount',
    params: ['0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21', 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const nonce = parseInt(response.data.result, 16);
console.log('Nonce:', nonce);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionCount',
        'params': ['0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21', 'latest'],
        'id': 'getblock.io'
    }
)

result = response.json()
nonce = int(result['result'], 16)
print(f'Nonce: {nonce}')
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
            "method": "eth_getTransactionCount",
            "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Nonce: {}", response["result"]);
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
    "result": "0x1a"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Transaction count (nonce) in hex        |

## Use Cases

{% stepper %}
{% step %}
### Transaction Nonce

Get nonce for signing new transactions.
{% endstep %}

{% step %}
### Pending Detection

Compare "latest" vs "pending" to detect pending transactions.
{% endstep %}

{% step %}
### Account Activity

Measure account transaction activity.
{% endstep %}

{% step %}
### Replay Prevention

Ensure unique nonce for each transaction.
{% endstep %}

{% step %}
### Transaction Management

Track outgoing transaction sequence.
{% endstep %}
{% endstepper %}

## Error Handling

| Error Code | Message        | Description                               |
| ---------- | -------------- | ----------------------------------------- |
| -32602     | Invalid params | Invalid address format or block parameter |
| -32603     | Internal error | Node internal failure                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getNonce(address) {
    // Get confirmed nonce
    const nonce = await provider.getTransactionCount(address);
    console.log('Confirmed Nonce:', nonce);
    
    // Get pending nonce (includes pending transactions)
    const pendingNonce = await provider.getTransactionCount(address, 'pending');
    console.log('Pending Nonce:', pendingNonce);
    
    // Check for pending transactions
    if (pendingNonce > nonce) {
        console.log('Pending Transactions:', pendingNonce - nonce);
    }
    
    return { nonce, pendingNonce };
}

// Build transaction with correct nonce
async function buildTransaction(wallet, to, value) {
    const nonce = await provider.getTransactionCount(wallet.address, 'pending');
    
    const tx = {
        to: to,
        value: ethers.parseEther(value),
        nonce: nonce,
        gasLimit: 21000n,
        maxFeePerGas: ethers.parseUnits('0.1', 'gwei'),
        maxPriorityFeePerGas: ethers.parseUnits('0.001', 'gwei'),
        chainId: 8453n
    };
    
    return tx;
}

getNonce('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getNonce(address) {
    // Get confirmed nonce
    const nonce = await client.getTransactionCount({
        address: address
    });
    console.log('Confirmed Nonce:', nonce);
    
    // Get pending nonce
    const pendingNonce = await client.getTransactionCount({
        address: address,
        blockTag: 'pending'
    });
    console.log('Pending Nonce:', pendingNonce);
    
    return { nonce, pendingNonce };
}

getNonce('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21');
```
{% endtab %}
{% endtabs %}
