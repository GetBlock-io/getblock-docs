---
description: >-
  Example code for the eth_getBlockByHash JSON RPC method. Сomplete guide on how
  to use eth_getBlockByHash JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - HyperEVM

This method returns information about a block by hash.

## Parameters

| Parameter        | Type    | Required | Description                                                                      |
| ---------------- | ------- | -------- | -------------------------------------------------------------------------------- |
| blockHash        | string  | Yes      | Hash of the block.                                                               |
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
    "method": "eth_getBlockByHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true],
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
    "method": "eth_getBlockByHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true],
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
    "method": "eth_getBlockByHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true],
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
    "method": "eth_getBlockByHash",
    "params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true],
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
        "number": "0x1b4",
        "hash": "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238",
        "parentHash": "0xa903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568237",
        "nonce": "0x0000000000000000",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0x00000000000000000000000000000000...",
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "miner": "0x0000000000000000000000000000000000000000",
        "difficulty": "0x0",
        "totalDifficulty": "0x0",
        "extraData": "0x",
        "size": "0x220",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0x0",
        "timestamp": "0x64a1b2c3",
        "transactions": [],
        "uncles": [],
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
| gasLimit      | string | Gas limit for block (hex).     |
| baseFeePerGas | string | EIP-1559 base fee (hex).       |

### Use Case

The `eth_getBlockByHash` method is essential for:

* Block explorer functionality
* Transaction confirmation verification
* Chain analysis and statistics
* Historical data retrieval
* Blockchain indexing systems

### Error Handling

| Error Code | Message        | Cause                          |
| ---------- | -------------- | ------------------------------ |
| -32602     | Invalid params | Invalid block hash format.     |
| -32603     | Internal error | Block not found or node issue. |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    const result = await provider.send("eth_getBlockByHash", ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true]);  
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
     method: "eth_getBlockByHash",
    params: ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238", true],
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
