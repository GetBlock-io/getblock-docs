---
description: >-
  Example code for the web3_sha3 json-rpc method. Сomplete guide on how to use
  web3_sha3 json-rpc in GetBlock.io Web3 documentation.
---

# web3\_sha3 - Optimism

This method computes and returns the Keccak-256 hash (NOT standard SHA3-256) of the input data. Keccak-256 is the hash function used throughout Ethereum and Optimism to compute addresses, transaction hashes, and other cryptographic values. The input must be a hexadecimal string.

### Parameters

| Parameter | Type | Description                                | Required |
| --------- | ---- | ------------------------------------------ | -------- |
| data      | DATA | The data to hash, as a hexadecimal string. | Yes      |

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": ["0x68656c6c6f"],
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
    params: ['0x68656c6c6f'],
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
    "params": ["0x68656c6c6f"],
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
        "params": ["0x68656c6c6f"],
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

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1c8aff950685c2ed4bc3174f3472287b56d9517b9c948127319a09a7a36deac8"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The Keccak-256 hash of the input data as a 32-byte hexadecimal string.

### Use Case

The web3\_sha3 method is commonly used for:

* **Data hashing**
* **Signature verification**
* **Address computation**
* **Cryptographic operations**

## Error Handling

| Error Code | Description                    |
| ---------- | ------------------------------ |
| -32602     | Invalid params - malformed hex |
| -32603     | Internal error                 |

## Web3 Integration

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
