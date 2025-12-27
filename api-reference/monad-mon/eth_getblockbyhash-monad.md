---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Monad

This method returns information about a block by its hash.

## Parameters

| Parameter        | Type    | Required | Description                                                                           |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------- |
| blockHash        | string  | Yes      | The 32-byte hash of a block.                                                          |
| fullTransactions | boolean | Yes      | If true, returns full transaction objects; if false, returns only transaction hashes. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x3dc04337840f64ba04b87c2f43a7a12f318180d3e9c4ac6227e711d5ec065be7", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x3dc04337840f64ba04b87c2f43a7a12f318180d3e9c4ac6227e711d5ec065be7", false],
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
{% code title="example.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x3dc04337840f64ba04b87c2f43a7a12f318180d3e9c4ac6227e711d5ec065be7", false],
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
            "method": "eth_getBlockByHash",
            "params": ["0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae", false],
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
        "hash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
        "parentHash": "0xe99e022112df268087ea7eafaf4790497fd21dbeeb6bd7a1721df161a6657a54",
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
        "gasUsed": "0x5208",
        "timestamp": "0x65a1c2b4",
        "transactions": ["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"],
        "uncles": [],
        "baseFeePerGas": "0x7"
    }
}
```
{% endcode %}

## Response Parameters

| Field            | Type   | Description                            |
| ---------------- | ------ | -------------------------------------- |
| number           | string | Block number in hex.                   |
| hash             | string | 32-byte block hash.                    |
| parentHash       | string | 32-byte parent block hash.             |
| nonce            | string | 8-byte nonce (always 0 for Monad PoS). |
| sha3Uncles       | string | SHA3 hash of uncles data.              |
| logsBloom        | string | 256-byte bloom filter for logs.        |
| transactionsRoot | string | Root of the transaction trie.          |
| stateRoot        | string | Root of the state trie.                |
| receiptsRoot     | string | Root of the receipts trie.             |
| miner            | string | Block producer address.                |
| difficulty       | string | Block difficulty (0 for PoS).          |
| totalDifficulty  | string | Cumulative difficulty.                 |
| extraData        | string | Extra data field.                      |
| size             | string | Block size in bytes.                   |
| gasLimit         | string | Maximum gas allowed.                   |
| gasUsed          | string | Total gas used.                        |
| timestamp        | string | Unix timestamp.                        |
| transactions     | array  | Transaction hashes or objects.         |
| baseFeePerGas    | string | Base fee per gas (EIP-1559).           |

## Use Case

The `eth_getBlockByHash` method is essential for:

* Block explorer functionality
* Transaction lookup by block
* Chain reorganization detection
* Historical data analysis
* Block verification
* Fork detection

## Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params     | Invalid block hash format.       |
| -32000      | Resource not found | Block not found.                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock("0x3dc04337840f64ba04b87c2f43a7a12f318180d3e9c4ac6227e711d5ec065be7");
console.log('Block number:', block.number);
console.log('Timestamp:', new Date(block.timestamp * 1000));
console.log('Transactions:', block.transactions.length);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="" overflow="wrap" %}
```javascript
import { createPublicClient, http } from "viem";
import { monad } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http(
    import.meta.env.MONAD_ACCESS_TOKEN
  ),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
     method: "eth_getBlockByHash",
    params: ["0x3dc04337840f64ba04b87c2f43a7a12f318180d3e9c4ac6227e711d5ec065be7", false],
    });
    console.log("Result:", result);
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
