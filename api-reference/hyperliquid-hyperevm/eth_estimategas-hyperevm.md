---
description: >-
  Example code for the eth_estimateGas JSON RPC method. Сomplete guide on how to
  use eth_estimateGas GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - HyperEVM

This method generates and returns an estimate of the amount of gas required to complete the transaction.

{% hint style="info" %}
When using estimates in production, always include a safety buffer (recommended 10–20%) to avoid out-of-gas failures due to state changes between estimation and submission.
{% endhint %}

## Parameters

| Parameter   | Type   | Required | Description                                |
| ----------- | ------ | -------- | ------------------------------------------ |
| transaction | object | Yes      | Transaction call object.                   |
| block       | string | No       | Block parameter (only "latest" supported). |

### Transaction Object

| Field    | Type   | Required | Description                 |
| -------- | ------ | -------- | --------------------------- |
| from     | string | No       | Sender address.             |
| to       | string | No       | Recipient/contract address. |
| gas      | string | No       | Gas limit (hex).            |
| gasPrice | string | No       | Gas price (hex).            |
| value    | string | No       | Value to send (hex).        |
| data     | string | No       | Transaction data (hex).     |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0xContractAddress",
        "data": "0xa9059cbb000000000000000000000000RecipientAddress0000000000000000000000000000000000000000000000000de0b6b3a7640000"
    }],
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
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0xContractAddress",
        "data": "0xa9059cbb000000000000000000000000RecipientAddress0000000000000000000000000000000000000000000000000de0b6b3a7640000"
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
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0xContractAddress",
        "data": "0xa9059cbb000000000000000000000000RecipientAddress0000000000000000000000000000000000000000000000000de0b6b3a7640000"
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
    "method": "eth_estimateGas",
    "params": [{
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0xContractAddress",
        "data": "0xa9059cbb000000000000000000000000RecipientAddress0000000000000000000000000000000000000000000000000de0b6b3a7640000"
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x5208"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| result | string | Estimated gas required (hex). |

## Use Case

The `eth_estimateGas` method is essential for:

* Setting appropriate gas limits for transactions
* Cost estimation before sending
* Transaction validation
* UI gas cost displays
* Preventing out-of-gas failures

## Error Handling

| Error Code | Message            | Cause                       |
| ---------- | ------------------ | --------------------------- |
| -32602     | Invalid params     | Invalid transaction object. |
| -32603     | Internal error     | Estimation failed.          |
| 3          | Execution reverted | Transaction would revert.   |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function estimateGas() {
    const gasEstimate = await provider.estimateGas({
        from: '0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21',
        to: '0xContractAddress',
        data: '0xa9059cbb...'
    });
    console.log('Gas Estimate:', gasEstimate.toString());
}

estimateGas();
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
      method: "eth_estimateGas",
      params: [
       {
         from: "0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb",
         to: "0x4bbeeb066ed09b7aed07bf39eee0460dfa261520",
         value: "0xde0b6b3a7640000",
       }]
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
