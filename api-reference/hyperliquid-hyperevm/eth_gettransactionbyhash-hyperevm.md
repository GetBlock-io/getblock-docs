---
description: >-
  Example code for the eth_getTransactionByHash JSON RPC method. Сomplete guide
  on how to use eth_getTransactionByHash GetBlock JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - HyperEVM

This method returns the information about a transaction requested by transaction hash.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | Hash of the transaction. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
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
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
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
            "method": "eth_getTransactionByHash",
            "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "blockNumber": "0x1b4",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "maxFeePerGas": "0x4a817c800",
        "maxPriorityFeePerGas": "0x0",
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "input": "0x",
        "nonce": "0x1",
        "to": "0x1234567890123456789012345678901234567890",
        "transactionIndex": "0x0",
        "value": "0xde0b6b3a7640000",
        "type": "0x2",
        "chainId": "0x3e7",
        "v": "0x0",
        "r": "0x...",
        "s": "0x..."
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type   | Description                          |
| -------------------- | ------ | ------------------------------------ |
| hash                 | string | Transaction hash.                    |
| blockHash            | string | Block hash (null if pending).        |
| blockNumber          | string | Block number (null if pending).      |
| from                 | string | Sender address.                      |
| to                   | string | Recipient address.                   |
| value                | string | Value transferred (hex wei).         |
| gas                  | string | Gas limit (hex).                     |
| gasPrice             | string | Gas price (hex wei).                 |
| maxFeePerGas         | string | EIP-1559 max fee (hex).              |
| maxPriorityFeePerGas | string | EIP-1559 priority fee (hex).         |
| nonce                | string | Sender nonce (hex).                  |
| input                | string | Transaction data (hex).              |
| type                 | string | Transaction type (0x2 for EIP-1559). |
| chainId              | string | Chain ID (hex).                      |

## Use Case

The `eth_getTransactionByHash` method is essential for:

* Transaction status tracking
* Payment verification
* Block explorer functionality
* Transaction debugging
* Wallet history displays

## Error Handling

| Error Code | Message        | Cause                     |
| ---------- | -------------- | ------------------------- |
| -32602     | Invalid params | Invalid transaction hash. |
| -32603     | Internal error | Transaction not found.    |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getTransaction() {
    const tx = await provider.getTransaction('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
    console.log('Transaction:', tx);
}

getTransaction();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.HYPER_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
    method: "eth_getTransactionByHash",
    params: ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
