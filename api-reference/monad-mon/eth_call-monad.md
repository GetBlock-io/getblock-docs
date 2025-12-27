---
description: >-
  Example code for the eth_call JSON-RPC method. Complete guide on how to use
  eth_call JSON-RPC in GetBlock Web3 documentation.
---

# eth\_call - Monad

This method executes a new message call immediately without creating a transaction on the blockchain. Useful for reading data from smart contracts.

## Parameters

| Parameter            | Type   | Required | Description                                                                   |
| -------------------- | ------ | -------- | ----------------------------------------------------------------------------- |
| transaction          | object | Yes      | The transaction call object.                                                  |
| transaction.from     | string | No       | The address the call is sent from.                                            |
| transaction.to       | string | Yes      | The address the call is directed to.                                          |
| transaction.gas      | string | No       | Gas provided for the call (hex).                                              |
| transaction.gasPrice | string | No       | Gas price in wei (hex).                                                       |
| transaction.value    | string | No       | Value sent with the call (hex).                                               |
| transaction.data     | string | No       | Hash of the method signature and encoded parameters.                          |
| blockNumber          | string | No       | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized". |

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
    "params": [{
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f0bEb"
    }, "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f0bEb"
    }, "latest"],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f0bEb"
    }, "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_call",
            "params": [{
                "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
                "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f0bEb"
            }, "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
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
    "result": "0x0000000000000000000000000000000000000000000000000000000005f5e100"
}
```

## Response Parameters

| Field  | Type   | Description                                       |
| ------ | ------ | ------------------------------------------------- |
| result | string | The return value of the executed contract method. |

## Use Case

The `eth_call` method is essential for:

* Reading smart contract state
* Checking token balances (ERC-20, ERC-721)
* Simulating transactions before execution
* Querying DeFi protocol data
* Fetching NFT metadata
* Price feed queries

## Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params     | Invalid call parameters.         |
| -32000      | Execution reverted | Contract execution reverted.     |
| -32000      | Out of gas         | Insufficient gas for execution.  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Read ERC-20 balance
const tokenContract = new ethers.Contract(tokenAddress, ['function balanceOf(address) view returns (uint256)'], provider);
const balance = await tokenContract.balanceOf(walletAddress);
console.log('Token balance:', ethers.formatUnits(balance, 18));
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { monad } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http("https://go.getblock.us/<ACCESS_TOKEN>"),
});

// Using the method through Viem
async function fetchAccountViem() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
            method: 'eth_blockNumber',
            params: [{
        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
        "data": "0x70a08231000000000000000000000000742d35Cc6634C0532925a3b844Bc9e7595f0bEb"
    }, "latest"],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}
```
{% endtab %}
{% endtabs %}

