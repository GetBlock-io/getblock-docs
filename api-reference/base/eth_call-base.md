---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Base

This method executes a new message call immediately without creating a transaction on the blockchain. This is the primary method for reading data from smart contracts, including token balances, contract state, and view/pure function results.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                 |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description                        |
| -------- | ------ | -------- | ---------------------------------- |
| from     | string | No       | 20-byte sender address             |
| to       | string | Yes      | 20-byte recipient/contract address |
| gas      | string | No       | Gas limit for the call (hex)       |
| gasPrice | string | No       | Gas price in wei (hex)             |
| value    | string | No       | Value to send in wei (hex)         |
| data     | string | No       | Encoded function call data         |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0x4200000000000000000000000000000000000006",
        "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

// ERC-20 balanceOf call
const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_call',
    params: [{
        to: '0x4200000000000000000000000000000000000006', // WETH on Base
        data: '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
    }, 'latest'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const balance = parseInt(response.data.result, 16);
console.log('Token Balance:', balance / 1e18);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_call',
        'params': [{
            'to': '0x4200000000000000000000000000000000000006',
            'data': '0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE'
        }, 'latest'],
        'id': 'getblock.io'
    }
)

result = response.json()
balance = int(result['result'], 16)
print(f'Token Balance: {balance / 1e18}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
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
            "method": "eth_call",
            "params": [{
                "to": "0x4200000000000000000000000000000000000006",
                "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
            }, "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
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
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded return data from the contract call |

## Use Cases

* Token Balances: Query ERC-20 token balances using balanceOf
* Contract State: Read public state variables from smart contracts
* Price Feeds: Query oracle prices from Chainlink or other protocols
* NFT Metadata: Read NFT ownership and metadata
* DeFi Calculations: Query pool reserves, exchange rates, and liquidity

## Error Handling

| Error Code | Message            | Description                                   |
| ---------- | ------------------ | --------------------------------------------- |
| -32602     | Invalid params     | Invalid transaction object or block parameter |
| -32603     | Internal error     | Contract execution error                      |
| 3          | Execution reverted | Contract reverted the call                    |
| -32000     | Execution error    | Insufficient gas or other execution failure   |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using Contract interface for type-safe calls
async function getTokenBalance(tokenAddress, walletAddress) {
    const abi = ['function balanceOf(address) view returns (uint256)'];
    const contract = new ethers.Contract(tokenAddress, abi, provider);
    
    const balance = await contract.balanceOf(walletAddress);
    console.log('Balance:', ethers.formatUnits(balance, 18));
    
    return balance;
}

// WETH on Base
getTokenBalance(
    '0x4200000000000000000000000000000000000006',
    '0x6E0d01A76C3Cf4288372a29124A26D4353EE51BE'
);

// Low-level call
async function rawCall(to, data) {
    const result = await provider.call({
        to: to,
        data: data
    });
    return result;
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, parseAbi } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using readContract for type-safe calls
async function getTokenBalance(tokenAddress, walletAddress) {
    const balance = await client.readContract({
        address: tokenAddress,
        abi: parseAbi(['function balanceOf(address) view returns (uint256)']),
        functionName: 'balanceOf',
        args: [walletAddress]
    });
    
    console.log('Balance:', balance);
    return balance;
}

// WETH on Base
getTokenBalance(
    '0x4200000000000000000000000000000000000006',
    '0x6E0d01A76C3Cf4288372a29124A26D4353EE51BE'
);

// Low-level call
async function rawCall(to, data) {
    const result = await client.call({
        to: to,
        data: data
    });
    return result;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
