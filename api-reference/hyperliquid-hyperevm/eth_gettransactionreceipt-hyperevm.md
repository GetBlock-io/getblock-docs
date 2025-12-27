---
description: >-
  Example code for the eth_getTransactionReceipt JSON RPC method. Сomplete guide
  on how to use eth_getTransactionReceipt GetBlock JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - HyperEVM

This method returns the receipt of a transaction by transaction hash.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | Hash of the transaction. |

## Request examples

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
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
    "method": "eth_getTransactionReceipt",
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
    "method": "eth_getTransactionReceipt",
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
        "method": "eth_getTransactionReceipt",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "transactionIndex": "0x0",
        "blockHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "blockNumber": "0x1b4",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0x1234567890123456789012345678901234567890",
        "cumulativeGasUsed": "0x5208",
        "gasUsed": "0x5208",
        "effectiveGasPrice": "0x3b9aca00",
        "contractAddress": null,
        "logs": [],
        "logsBloom": "0x00000000...",
        "status": "0x1",
        "type": "0x2"
    }
}
```

## Response Parameters

| Field             | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| transactionHash   | string | Transaction hash.                          |
| blockHash         | string | Block hash.                                |
| blockNumber       | string | Block number (hex).                        |
| from              | string | Sender address.                            |
| to                | string | Recipient address.                         |
| status            | string | 0x1 (success) or 0x0 (failure).            |
| gasUsed           | string | Gas used (hex).                            |
| effectiveGasPrice | string | Actual gas price paid (hex).               |
| contractAddress   | string | Contract address if deployment, else null. |
| logs              | array  | Array of log objects.                      |
| type              | string | Transaction type.                          |

## Use Case

The `eth_getTransactionReceipt` method is essential for:

* Confirming transaction success/failure
* Retrieving emitted events
* Getting deployed contract addresses
* Calculating actual gas costs
* Payment confirmation systems

## Error Handling

| Error Code | Message        | Cause                     |
| ---------- | -------------- | ------------------------- |
| -32602     | Invalid params | Invalid transaction hash. |
| -32603     | Internal error | Receipt not found.        |

Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getReceipt() {
    const receipt = await provider.getTransactionReceipt('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');
    console.log('Status:', receipt.status === 1 ? 'Success' : 'Failed');
    console.log('Gas Used:', receipt.gasUsed.toString());
}

getReceipt();
```
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
     method: "eth_getTransactionReceipt",
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
