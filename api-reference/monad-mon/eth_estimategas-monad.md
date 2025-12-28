---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Monad

This method generates an estimate of how much gas is needed for a transaction to complete. The transaction will not be added to the blockchain.

## Parameters

| Parameter            | Type   | Required | Description                                              |
| -------------------- | ------ | -------- | -------------------------------------------------------- |
| transaction          | object | Yes      | The transaction call object.                             |
| transaction.from     | string | No       | The address the transaction is sent from.                |
| transaction.to       | string | No       | The address the transaction is directed to.              |
| transaction.gas      | string | No       | Upper limit of gas for the transaction (hex).            |
| transaction.gasPrice | string | No       | Gas price in wei (hex).                                  |
| transaction.value    | string | No       | Value sent with the transaction (hex).                   |
| transaction.data     | string | No       | Hash of the method signature and encoded parameters.     |
| blockNumber          | string | No       | Block number in hex, or "latest", "earliest", "pending". |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
        "value": "0xde0b6b3a7640000"
    }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="estimateGas.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
        "value": "0xde0b6b3a7640000"
    }],
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
{% code title="estimate_gas.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
        "to": "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
        "value": "0xde0b6b3a7640000"
    }],
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
{% code title="main.rs" %}
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
            "method": "eth_estimateGas",
            "params": [{
                "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
                "to": "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
                "value": "0xde0b6b3a7640000"
            }],
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
    "result": "0x5208"
}
```

## Response Parameters

| Field  | Type   | Description                                                                      |
| ------ | ------ | -------------------------------------------------------------------------------- |
| result | string | The estimated gas required in hexadecimal (21000 = 0x5208 for simple transfers). |

## Use Case

The `eth_estimateGas` method is essential for:

* Calculating transaction gas limits
* Cost estimation before transactions
* Smart contract interaction planning
* Wallet gas limit suggestions
* DeFi transaction preparation
* Batch transaction planning

## Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params     | Invalid transaction parameters.  |
| -32000      | Execution reverted | Transaction would revert.        |
| -32000      | Out of gas         | Gas limit insufficient.          |
| -32000      | Insufficient funds | Not enough MON for value + gas.  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-estimate.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const tx = {
    from: senderAddress,
    to: recipientAddress,
    value: ethers.parseEther('1.0'),
    data: '0x'
};

const gasEstimate = await provider.estimateGas(tx);
console.log('Estimated gas:', gasEstimate.toString());

// Add buffer for safety
const gasLimit = gasEstimate * 120n / 100n; // 20% buffer
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="" %}
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
            method: 'eth_estimateGas',
            param: [{
    from: senderAddress,
    to: recipientAddress,
    value: ethers.parseEther('1.0'),
    data: '0x'
}],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }

```
{% endcode %}
{% endtab %}
{% endtabs %}
