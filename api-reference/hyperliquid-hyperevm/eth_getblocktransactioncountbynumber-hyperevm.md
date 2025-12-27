---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber JSON RPC method.
  Сomplete guide on how to use eth_getBlockTransactionCountByNumber GetBlock
  JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - HyperEVM

This method returns the number of transactions in a block matching the given block number.

## Parameters

| Parameter   | Type   | Required | Description                                            |
| ----------- | ------ | -------- | ------------------------------------------------------ |
| blockNumber | string | Yes      | Block number (hex) or "latest", "earliest", "pending". |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByNumber',
    params: ['latest'],
    id: 'getblock.io'
});

console.log('Transaction Count:', parseInt(response.data.result, 16));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getBlockTransactionCountByNumber',
    'params': ['latest'],
    'id': 'getblock.io'
})

result = response.json()['result']
print(f'Transaction Count: {int(result, 16)}')
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
            "method": "eth_getBlockTransactionCountByNumber",
            "params": ["latest"],
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
    "result": "0x5"
}
```

## Response Parameters

| Field  | Type   | Description                           |
| ------ | ------ | ------------------------------------- |
| result | string | Transaction count in the block (hex). |

## Use Case

The `eth_getBlockTransactionCountByNumber` method is essential for:

* Block explorer statistics
* Transaction throughput analysis
* Network activity monitoring
* Block verification

## Error Handling

| Error Code | Message        | Cause                        |
| ---------- | -------------- | ---------------------------- |
| -32602     | Invalid params | Invalid block number format. |
| -32603     | Internal error | Block not found.             |

### Web3 Integration&#x20;

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
    const result = await provider.send("eth_getBlockTransactionCountByNumber", ["latest"])
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
     method: "eth_getBlockTransactionCountByNumber",
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
