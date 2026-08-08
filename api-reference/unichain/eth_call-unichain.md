---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide to using the
  eth_call JSON-RPC method in the GetBlock.io Web3 documentation.
---

# eth\_call - Unichain

This method executes a new message call immediately without creating a transaction on the blockchain. This is the primary method for reading data from smart contracts, including token balances, contract state, and view or pure function results.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| transaction    | object | Yes      | Transaction call object                                 |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

### Transaction Object

| Field    | Type   | Required | Description                           |
| -------- | ------ | -------- | ------------------------------------- |
| from     | string | No       | 20-byte sender address                |
| to       | string | Yes      | 20-byte recipient or contract address |
| gas      | string | No       | Gas limit for the call (hex)          |
| gasPrice | string | No       | Gas price in wei (hex)                |
| value    | string | No       | Value to send in wei (hex)            |
| data     | string | No       | Encoded function call data            |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"}, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_call',
    params: [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"}, "latest"],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
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
        'params': [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"}, "latest"],
        'id': 'getblock.io'
    }
)

print(response.json())
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
            "params": [{"to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"}, "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000008ac7230489e80000"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded return data from the contract call |

## Use Cases

* **Token Balances**: Query ERC-20 token balances using balanceOf
* **Contract State**: Read public state variables from smart contracts
* **Price Feeds**: Query oracle prices from Chainlink or other protocols
* **DeFi Calculations**: Query pool reserves, exchange rates, and liquidity
* **NFT Metadata**: Read NFT ownership and token metadata

## Error Handling

| Error Code | Message            | Description                                   |
| ---------- | ------------------ | --------------------------------------------- |
| -32602     | Invalid params     | Invalid transaction object or block parameter |
| -32603     | Internal error     | Contract execution error                      |
| 3          | Execution reverted | The contract reverted the call                |
| -32000     | Execution error    | Insufficient gas or other execution failure   |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const abi = ['function balanceOf(address) view returns (uint256)'];
const token = new ethers.Contract('0x4200000000000000000000000000000000000006', abi, provider);
const balance = await token.balanceOf('0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045');
console.log(ethers.formatUnits(balance, 18));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { unichain } from 'viem/chains';

const client = createPublicClient({ chain: unichain, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const balance = await client.readContract({
    address: '0x4200000000000000000000000000000000000006',
    abi: [{ name: 'balanceOf', type: 'function', stateMutability: 'view',
            inputs: [{ name: 'account', type: 'address' }], outputs: [{ type: 'uint256' }] }],
    functionName: 'balanceOf',
    args: ['0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045']
});
console.log(balance);
```
{% endcode %}
{% endtab %}
{% endtabs %}
