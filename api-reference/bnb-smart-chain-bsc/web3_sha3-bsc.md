---
description: >-
  Example code for the web3_sha3 JSON RPC method. Сomplete guide on how to use
  web3_sha3 JSON RPC in GetBlock Web3 documentation.
---

# web3\_sha3 - BSC

This method returns the Keccak-256 hash of the given data. This is the same hashing algorithm used by Ethereum and BSC for addresses and signatures.

## Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| data      | string | Yes      | Data to hash (hex encoded) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": ["0x68656c6c6f20776f726c64"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// "hello world" in hex
const payload = {
    jsonrpc: '2.0',
    method: 'web3_sha3',
    params: ['0x68656c6c6f20776f726c64'],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log('Hash:', response.data.result))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": ["0x68656c6c6f20776f726c64"],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(f"Hash: {response.json()['result']}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "web3_sha3",
        "params": ["0x68656c6c6f20776f726c64"],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="Response" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description     |
| --------- | ------ | --------------- |
| result    | string | Keccak-256 hash |

## Use Cases

* Compute function selectors
* Generate event topic hashes
* Verify data integrity
* Create deterministic identifiers

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32602     | Invalid params - malformed hex |
| -32603     | Internal error                 |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

// Compute keccak256 locally
const hash = ethers.keccak256(ethers.toUtf8Bytes('hello world'));
console.log('Hash:', hash);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { keccak256, toHex } from 'viem';

const hash = keccak256(toHex('hello world'));
console.log('Hash:', hash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
