---
description: >-
  Example code for the eth_getBlockByNumber JSON RPC method. Сomplete guide on
  how to use eth_getBlockByNumber JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - HyperEVM

This method returns information about a block by block number.

## Parameters

| Parameter        | Type    | Required | Description                                                                      |
| ---------------- | ------- | -------- | -------------------------------------------------------------------------------- |
| blockNumber      | string  | Yes      | Block number (hex) or "latest", "earliest", "pending".                           |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns transaction hashes. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
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
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
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
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
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
            "method": "eth_getBlockByNumber",
            "params": ["latest", true],
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
        "number": "0x1b4",
        "hash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "parentHash": "0xa903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568237",
        "timestamp": "0x64a1b2c3",
        "transactions": [],
        "gasUsed": "0x0",
        "gasLimit": "0x1c9c380",
        "baseFeePerGas": "0x3b9aca00"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| number        | string | Block number (hex).            |
| hash          | string | Block hash.                    |
| parentHash    | string | Parent block hash.             |
| timestamp     | string | Block timestamp (hex).         |
| transactions  | array  | Transaction hashes or objects. |
| gasUsed       | string | Gas used in block (hex).       |
| baseFeePerGas | string | EIP-1559 base fee (hex).       |

## Use Case

The `eth_getBlockByNumber` method is essential for:

* Block explorer functionality
* Chain synchronization tracking
* Sequential block processing
* Historical data analysis
* Real-time block monitoring

## Error Handling

| Error Code | Message        | Cause                          |
| ---------- | -------------- | ------------------------------ |
| -32602     | Invalid params | Invalid block number format.   |
| -32603     | Internal error | Block not found or node issue. |

Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    const result = await provider.send("eth_getBlockByNumber", ["latest", true]);  
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}

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
    "method": "eth_getBlockByNumber",
    "params": ["latest", true],
};
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
