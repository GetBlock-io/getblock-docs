---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionByHash - Monad

This method returns information about a transaction by its hash.

## Parameters

| Parameter       | Type   | Required | Description                   |
| --------------- | ------ | -------- | ----------------------------- |
| transactionHash | string | Yes      | The 32-byte transaction hash. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8"],
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
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8"],
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
            "method": "eth_getTransactionByHash",
            "params": ["0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8"],
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
        "blockHash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
        "blockNumber": "0x1b4",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f0beb",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "maxFeePerGas": "0x3b9aca00",
        "maxPriorityFeePerGas": "0x3b9aca00",
        "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "input": "0x",
        "nonce": "0x1a",
        "to": "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
        "transactionIndex": "0x0",
        "value": "0xde0b6b3a7640000",
        "type": "0x2",
        "chainId": "0x8f",
        "v": "0x1",
        "r": "0x...",
        "s": "0x..."
    }
}
```
{% endcode %}

### Response Parameters Definition

| Field                | Type   | Description                                                    |
| -------------------- | ------ | -------------------------------------------------------------- |
| blockHash            | string | Hash of the block containing the transaction. Null if pending. |
| blockNumber          | string | Block number. Null if pending.                                 |
| from                 | string | Address of the sender.                                         |
| gas                  | string | Gas provided by the sender.                                    |
| gasPrice             | string | Gas price in MON-wei.                                          |
| maxFeePerGas         | string | Maximum fee per gas (EIP-1559).                                |
| maxPriorityFeePerGas | string | Maximum priority fee per gas (EIP-1559).                       |
| hash                 | string | Transaction hash.                                              |
| input                | string | Data sent with the transaction.                                |
| nonce                | string | Number of transactions from sender prior to this one.          |
| to                   | string | Address of the receiver. Null for contract creation.           |
| transactionIndex     | string | Index position in the block. Null if pending.                  |
| value                | string | Value transferred in MON-wei.                                  |
| type                 | string | Transaction type (0x0=legacy, 0x1=access list, 0x2=EIP-1559).  |
| chainId              | string | Chain ID (0x8f = 143 for Monad Mainnet).                       |
| v, r, s              | string | ECDSA signature values.                                        |

## Use Case

The `eth_getTransactionByHash` method is essential for:

* Transaction status checking
* Block explorer functionality
* Transaction receipt lookup
* Payment verification
* DeFi transaction tracking
* Wallet transaction history

### Error handling

| Status Code | Error Message  | Cause                            |
| ----------- | -------------- | -------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params | Invalid transaction hash format. |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const tx = await provider.getTransaction("0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8");

if (tx) {
    console.log('From:', tx.from);
    console.log('To:', tx.to);
    console.log('Value:', ethers.formatEther(tx.value), 'MON');
    console.log('Block:', tx.blockNumber);
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { monad } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http(
    import.meta.env.MONAD_ACCESS_TOKEN
  ),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
     method: "eth_getTransactionByHash",
    params: ["0x2623a9878543c1b8a1e1c9f6cd58bcde5958edeed4b90c616f03f41e99faa1f8"],
    });
    console.log("Result:", result);
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}

```
{% endtab %}
{% endtabs %}
