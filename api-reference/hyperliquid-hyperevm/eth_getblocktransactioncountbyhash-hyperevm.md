---
description: >-
  Example code for the eth_getBlockTransactionCountByHash JSON RPC method.
  Сomplete guide on how to use eth_getBlockTransactionCountByHash GetBlock JSON
  RPC in GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - HyperEVM

This method returns the number of transactions in a block that matches the given block hash.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| blockHash | string | Yes      | Hash of the block. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByHash',
    params: ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'],
    id: 'getblock.io'
});

console.log('Transaction Count:', parseInt(response.data.result, 16));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getBlockTransactionCountByHash',
    'params': ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'],
    'id': 'getblock.io'
})

result = response.json()['result']
print(f'Transaction Count: {int(result, 16)}')
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
            "method": "eth_getBlockTransactionCountByHash",
            "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],
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
    "result": "0xa"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| result | string | Transaction count in the block (hex). |

## Use Case

The `eth_getBlockTransactionCountByHash` method is essential for:

* Block explorer statistics
* Transaction density analysis
* Block verification
* Network activity monitoring

## Error Handling

<details>

<summary>Error Handling</summary>



</details>

| Error Code | Message        | Cause                      |
| ---------- | -------------- | -------------------------- |
| -32602     | Invalid params | Invalid block hash format. |
| -32603     | Internal error | Block not found.           |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    const result = await provider.send("eth_getBlockTransactionCountByHash",
    ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],);
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
      method: "eth_getBlockTransactionCountByHash",
      params: ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],
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
