---
description: >-
  Example code for the eth_bigBlockGasPrice JSON RPC method. Сomplete guide on
  how to use eth_bigBlockGasPrice GetBlock JSON RPC in GetBlock Web3
  documentation.
---

# eth\_bigBlockGasPrice - HyperEVM

This method returns the gas price for the next big block. This is a HyperEVM-specific method.

{% hint style="info" %}
This method returns the gas price specifically for the next big block. If you need the small block gas price use `eth_gasPrice`.
{% endhint %}

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_bigBlockGasPrice",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_bigBlockGasPrice',
    params: [],
    id: 'getblock.io'
});

console.log('Big Block Gas Price:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_bigBlockGasPrice',
    'params': [],
    'id': 'getblock.io'
})

result = response.json()['result']
print(f'Big Block Gas Price: {int(result, 16)} wei')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
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
            "method": "eth_bigBlockGasPrice",
            "params": [],
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
    "result": "0x3b9aca00"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                |
| ------ | ------ | ------------------------------------------ |
| result | string | Gas price for next big block in wei (hex). |

## Use Case

The `eth_bigBlockGasPrice` method is essential for:

* Pricing transactions for big blocks
* High-throughput transaction planning
* Fee estimation for bulk operations
* Optimizing gas costs for large operations

### Erro handling

| Error Code | Message        | Cause       |
| ---------- | -------------- | ----------- |
| -32603     | Internal error | Node issue. |

### Web3 Integration&#x20;

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_bigBlockGasPrice", []);
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
      method: "eth_bigBlockGasPrice",
      params: [],
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

