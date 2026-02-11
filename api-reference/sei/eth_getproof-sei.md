---
description: >-
  Example code for the eth_getProof JSON-RPC method. Complete guide on how to
  use eth_getProof JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getProof - SEI

Returns the IAVL proof (note: not a MPT proof) of the given keys for an account on Sei. This is useful for verifying account state.

## Parameters

| Parameter   | Type   | Description                                                                  |
| ----------- | ------ | ---------------------------------------------------------------------------- |
| address     | string | The address of the account                                                   |
| storageKeys | array  | Array of storage keys to prove                                               |
| block       | string | Block number in hex, or 'latest', 'earliest', 'pending', 'safe', 'finalized' |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_getProof',
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getProof",
            "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"],
            "id": "getblock.io"
        }))
        .send()
        .await?;
    
    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "address": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21",
        "accountProof": ["0x..."],
        "balance": "0x2386f26fc10000",
        "codeHash": "0xc5d2...",
        "nonce": "0x1",
        "storageHash": "0x56e8...",
        "storageProof": []
    }
}
```
{% endcode %}

## Response Parameters

| Field        | Type   | Description                   |
| ------------ | ------ | ----------------------------- |
| address      | string | The requested address         |
| accountProof | array  | Array of proof nodes          |
| balance      | string | The balance in wei            |
| codeHash     | string | Hash of the account's code    |
| nonce        | string | The account's nonce           |
| storageHash  | string | Hash of the storage trie root |
| storageProof | array  | Array of storage proofs       |

## Use Case

The `eth_getProof` method is essential for:

* Blockchain developers building applications on Sei
* Wallet applications requiring network data
* Analytics platforms tracking Sei network activity
* DeFi protocols integrating with Sei's parallelized EVM

## Error Handling

Common errors when using this method:

| Error Code | Message          | Description                                |
| ---------- | ---------------- | ------------------------------------------ |
| -32700     | Parse error      | Invalid JSON                               |
| -32600     | Invalid Request  | JSON is not a valid request object         |
| -32601     | Method not found | Method does not exist                      |
| -32602     | Invalid params   | Invalid method parameters                  |
| -32603     | Internal error   | Internal JSON-RPC error                    |
| -32000     | Invalid input    | Generic input error                        |
| -32500     | Cross-VM error   | Error in cross-VM operation (Sei-specific) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using ethers provider methods
const result = await provider.send('eth_getProof', ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sei } from 'viem/chains';

const client = createPublicClient({
    chain: sei,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using viem's request method
const result = await client.request({
    method: 'eth_getProof',
    params: ["0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", ["0x0"], "latest"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
