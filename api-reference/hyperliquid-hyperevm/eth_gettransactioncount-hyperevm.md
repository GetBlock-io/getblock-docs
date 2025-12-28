---
description: >-
  Example code for the eth_getTransactionCount JSON RPC method. Сomplete guide
  on how to use _eth_getTransactionCount GetBlock JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionCount - HyperEVM

This method returns the number of transactions sent from an address (nonce).

## Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| address   | string | Yes      | Address to check.                          |
| block     | string | Yes      | Block parameter (only "latest" supported). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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

payload = json.dumps(
    "jsonrpc": "2.0",
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
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
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Ethers.js)" %}
{% code title="JavaScript (Ethers.js)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getNonce() {
    const nonce = await provider.getTransactionCount('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21');
    console.log('Nonce:', nonce);
}

getNonce();
```
{% endcode %}
{% endtab %}

{% tab title="Python (Web3.py)" %}
{% code title="Python (Web3.py)" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider('https://go.getblock.io/<ACCESS-TOKEN>/'))

nonce = w3.eth.get_transaction_count('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21')
print(f'Nonce: {nonce}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
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
            "method": "eth_getTransactionCount",
            "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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

{% code title="Response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1a"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                    |
| ------ | ------ | ------------------------------ |
| result | string | Transaction count/nonce (hex). |

## Use Case

The `eth_getTransactionCount` method is essential for:

* Setting nonce for new transactions
* Transaction sequencing
* Detecting pending transactions
* Account activity analysis
* Preventing nonce collisions

## Error Handling

| Error Code | Message        | Cause                   |
| ---------- | -------------- | ----------------------- |
| -32602     | Invalid params | Invalid address format. |
| -32603     | Internal error | Node issue.             |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getNonce() {
    const nonce = await provider.getTransactionCount('0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21');
    console.log('Nonce:', nonce);
}

getNonce();
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
      method: "eth_getTransactionCount",
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "latest"],
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



