---
description: >-
  Example code for the eth_getBlockReceipts JSON RPC method. Сomplete guide on
  how to use eth_getBlockReceipts GetBlock JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getBlockReceipts - HyperEVM

This method returns all transaction receipts for a given block.

## Parameters

| Parameter   | Type   | Required | Description                     |
| ----------- | ------ | -------- | ------------------------------- |
| blockNumber | string | Yes      | Block number (hex) or "latest". |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (axios)" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockReceipts',
    params: ['latest'],
    id: 'getblock.io'
});

console.log('Receipts:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getBlockReceipts',
    'params': ['latest'],
    'id': 'getblock.io'
})

print(f'Receipts: {response.json()["result"]}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
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
            "method": "eth_getBlockReceipts",
            "params": ["latest"],
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

{% code title="Example response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "transactionHash": "0x...",
            "blockNumber": "0x1b4",
            "from": "0x...",
            "to": "0x...",
            "status": "0x1",
            "gasUsed": "0x5208",
            "logs": []
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type  | Description                           |
| ------ | ----- | ------------------------------------- |
| result | array | Array of transaction receipt objects. |

## Use Case

The `eth_getBlockReceipts` method is essential for:

* Batch processing block transactions
* Block indexing systems
* Event log aggregation
* Transaction analysis per block
* Efficient data retrieval

## Error Handling

| Error Code | Message        | Cause                 |
| ---------- | -------------- | --------------------- |
| -32602     | Invalid params | Invalid block number. |
| -32603     | Internal error | Block not found.      |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/ACCESS_TOKEN";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    const result = await provider.send("eth_getBlockReceipts",
    ["latest"]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
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
     method: "eth_getBlockReceipts",
      params: ["latest"],
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

