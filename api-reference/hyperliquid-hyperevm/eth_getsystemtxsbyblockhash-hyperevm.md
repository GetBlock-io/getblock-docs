---
description: >-
  Example code for the eth_getSystemTxsByBlockNumber JSON RPC method. Сomplete
  guide on how to use eth_getSystemTxsByBlockNumber GetBlock JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getSystemTxsByBlockHash - HyperEVM

This method returns system transactions that originate from HyperCore for a given block hash. This is a HyperEVM-specific method.

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
    "method": "eth_getSystemTxsByBlockHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getSystemTxsByBlockHash',
    params: ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'],
    id: 'getblock.io'
});

console.log('System Transactions:', response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="request.py" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'method': 'eth_getSystemTxsByBlockHash',
    'params': ['0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'],
    'id': 'getblock.io'
})

print(f'System Transactions: {response.json()["result"]}')
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
            "method": "eth_getSystemTxsByBlockHash",
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
    "result": [
        {
            "hash": "0x...",
            "blockHash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
            "blockNumber": "0x1b4",
            "from": "0x0000000000000000000000000000000000000000",
            "to": "0x...",
            "value": "0x0",
            "input": "0x...",
            "type": "system"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type  | Description                                         |
| ------ | ----- | --------------------------------------------------- |
| result | array | Array of system transaction objects from HyperCore. |

### System Transaction Object

| Field       | Type   | Description                                         |
| ----------- | ------ | --------------------------------------------------- |
| hash        | string | Transaction hash.                                   |
| blockHash   | string | Block hash.                                         |
| blockNumber | string | Block number (hex).                                 |
| from        | string | Origin address (often zero address for system txs). |
| to          | string | Target address.                                     |
| value       | string | Value transferred (hex).                            |
| input       | string | Transaction data (hex).                             |
| type        | string | Transaction type ("system").                        |

## Use Case

The `eth_getSystemTxsByBlockHash` method is essential for:

* Tracking HyperCore to HyperEVM interactions
* Monitoring cross-layer system operations
* Indexing system-level state changes
* Auditing HyperCore-triggered events
* Building comprehensive block explorers

## Error Handling

| Error Code | Message        | Cause                      |
| ---------- | -------------- | -------------------------- |
| -32602     | Invalid params | Invalid block hash format. |
| -32603     | Internal error | Block not found.           |

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
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_getSystemTxsByBlockHash", ["0x174ae649105c174167991dab0748b9cae84c91ab6d52d598a1118651feccbc4b"]);
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
      method: "eth_getSystemTxsByBlockHash",
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
