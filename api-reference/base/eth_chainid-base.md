---
description: >-
  Example code for the eth_chainId JSON-RPC method. Complete guide on how to use
  eth_chainId JSON-RPC in GetBlock Web3 documentation.
---

# eth\_chainId - Base

The `eth_chainId` method returns the chain ID of the Base network. This value is essential for EIP-155 transaction signing to prevent replay attacks across different networks. Base mainnet uses chain ID 8453.

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
    "method": "eth_chainId",
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
    method: 'eth_chainId',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Chain ID:', parseInt(response.data.result, 16));
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
        'method': 'eth_chainId',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
chain_id = int(result['result'], 16)
print(f'Chain ID: {chain_id}')
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
            "method": "eth_chainId",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Chain ID: {}", response["result"]);
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
    "result": "0x2105"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                     |
| id        | string | Request identifier matching the request               |
| result    | string | Hexadecimal chain ID (0x2105 = 8453 for Base mainnet) |

### Chain ID Reference

| Network      | Chain ID (Decimal) | Chain ID (Hex) |
| ------------ | -----------------: | -------------- |
| Base Mainnet |               8453 | 0x2105         |
| Base Sepolia |              84532 | 0x14A34        |

## Use Cases

* Network Verification: Confirm connection to the correct network before transactions
* Transaction Signing: Include chain ID in EIP-155 signatures to prevent replay attacks
* Multi-Chain Applications: Route transactions to appropriate networks
* Wallet Integration: Verify network configuration matches expected chain
* Smart Contract Deployment: Ensure deployment targets the intended network

## Error Handling

| Error Code | Message         | Description                |
| ---------- | --------------- | -------------------------- |
| -32603     | Internal error  | Node internal failure      |
| -32600     | Invalid request | Malformed JSON-RPC request |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getChainId() {
    const network = await provider.getNetwork();
    console.log('Chain ID:', network.chainId);
    console.log('Network Name:', network.name);
    return network.chainId;
}

getChainId();
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

async function getChainId() {
    const chainId = await client.getChainId();
    console.log('Chain ID:', chainId);
    return chainId;
}

getChainId();
```
{% endcode %}
{% endtab %}
{% endtabs %}
