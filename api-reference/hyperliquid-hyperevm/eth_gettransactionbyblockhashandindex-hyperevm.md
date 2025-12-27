---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex GetBlock
  JSON RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - HyperEVM

This method returns information about a transaction by block hash and transaction index position.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| blockHash | string | Yes      | Hash of the block.                |
| index     | string | Yes      | Transaction index position (hex). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", "0x0"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="JavaScript" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238', '0x0'],
    id: 'getblock.io'
});

console.log('Transaction:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getTransactionByBlockHashAndIndex',
    'params': ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238', '0x0'],
    'id': 'getblock.io'
})

print(f'Transaction: {response.json()["result"]}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
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
            "method": "eth_getTransactionByBlockHashAndIndex",
            "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", "0x0"],
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

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "blockHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "blockNumber": "0x1b4",
        "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "to": "0x1234567890123456789012345678901234567890",
        "value": "0xde0b6b3a7640000",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "nonce": "0x1",
        "transactionIndex": "0x0"
    }
}
```
{% endcode %}

## Response Parameters

| Field            | Type   | Description                  |
| ---------------- | ------ | ---------------------------- |
| hash             | string | Transaction hash.            |
| blockHash        | string | Block hash.                  |
| blockNumber      | string | Block number (hex).          |
| from             | string | Sender address.              |
| to               | string | Recipient address.           |
| value            | string | Value transferred (hex wei). |
| transactionIndex | string | Index in block (hex).        |

## Use Case

The `eth_getTransactionByBlockHashAndIndex` method is essential for:

* Sequential block transaction processing
* Block indexing
* Transaction verification by position
* Block explorer functionality

## Error Handling

| Error Code | Message        | Cause                        |
| ---------- | -------------- | ---------------------------- |
| -32602     | Invalid params | Invalid block hash or index. |
| -32603     | Internal error | Transaction not found.       |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/ca242756857342b79838d95d8c848a8d";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_getTransactionByBlockHashAndIndex",
[
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
   "0x0"])
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
{% code overflow="wrap" %}
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
     method: "eth_getTransactionByBlockHashAndIndex",
     params: [
       "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
       "0x0",
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

