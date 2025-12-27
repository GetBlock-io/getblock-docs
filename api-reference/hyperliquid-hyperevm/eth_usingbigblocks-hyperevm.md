---
description: >-
  Example code for the eth_usingBigBlocks JSON RPC method. Сomplete guide on how
  to use eth_usingBigBlocks GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_usingBigBlocks - HyperEVM

This method returns whether the specified address is using big blocks. This is a HyperEVM-specific method.

{% hint style="warning" %}
This is a HyperEVM-specific method (not standard Ethereum).
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description           |
| --------- | ------ | -------- | --------------------- |
| address   | string | Yes      | The address to check. |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_usingBigBlocks",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_usingBigBlocks',
    params: ['0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21'],
    id: 'getblock.io'
});

console.log('Using Big Blocks:', response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_usingBigBlocks',
    'params': ['0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21'],
    'id': 'getblock.io'
})

print(f'Using Big Blocks: {response.json()["result"]}')
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "eth_usingBigBlocks",
            "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```

## Response Parameters

| Field  | Type    | Description                                               |
| ------ | ------- | --------------------------------------------------------- |
| result | boolean | `true` if address is using big blocks, `false` otherwise. |

## Use Case

The `eth_usingBigBlocks` method is essential for:

* Determining appropriate gas pricing strategy
* Checking address block mode configuration
* Fee estimation for specific addresses
* High-throughput application configuration

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
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_usingBigBlocks", ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21"]);
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
      method: "eth_usingBigBlocks",
      params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21"],
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
