---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex GetBlock
  JSON RPC in GetBlock Web3 documentation.
---

# eth\_maxPriorityFeePerGas - HyperEVM

This method returns a suggestion for the max priority fee per gas for EIP-1559 transactions.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
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
    method: 'eth_maxPriorityFeePerGas',
    params: [],
    id: 'getblock.io'
});

console.log('Max Priority Fee:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_maxPriorityFeePerGas',
    'params': [],
    'id': 'getblock.io'
})

print(f'Max Priority Fee: {response.json()["result"]}')
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
            "method": "eth_maxPriorityFeePerGas",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

## Response Parameters

| Field  | Type   | Description                                   |
| ------ | ------ | --------------------------------------------- |
| result | string | Suggested max priority fee per gas (hex wei). |

## Use Case

The `eth_maxPriorityFeePerGas` method is essential for:

* EIP-1559 transaction fee estimation
* Setting appropriate priority fees
* Transaction priority optimization
* Fee strategy implementation

## Error Handling

| Error Code | Message        | Cause      |
| ---------- | -------------- | ---------- |
| -32603     | Internal error | Node issue |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_maxPriorityFeePerGas", []);
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
      method: "eth_maxPriorityFeePerGas",
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
