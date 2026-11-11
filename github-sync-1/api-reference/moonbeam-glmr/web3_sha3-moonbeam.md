---
description: >-
  Example code for the web3_sha3 JSON-RPC method. Complete guide on how to use
  web3_sha3 JSON-RPC in GetBlock Web3 documentation.
---

# web3\_sha3 - Moonbeam

This method returns the Keccak-256 (SHA3) hash of the given hex-encoded data. It is used to compute function selectors and event topic hashes without a local crypto library.

## Parameters

| Parameter | Type   | Required | Description                            |
| --------- | ------ | -------- | -------------------------------------- |
| data      | string | Yes      | Hex-encoded data to hash (0x-prefixed) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
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
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'web3_sha3',
    params: ['0x68656c6c6f20776f726c64'],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'web3_sha3',
        'params': ['0x68656c6c6f20776f726c64'],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "web3_sha3",
            "params": ["0x68656c6c6f20776f726c64"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | string | 32-byte Keccak-256 hash of the input data |

## Use Cases

* **Function Selectors**: Derive the 4-byte selector of a Solidity function signature
* **Event Topics**: Compute the topic0 hash for an event signature used in log filters
* **Data Integrity**: Hash arbitrary calldata to verify it matches an expected digest
* **Off-Chain Parity**: Cross-check a locally computed Keccak-256 hash against the node

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32602     | Invalid params | Input is not valid 0x-prefixed hex data |
| -32603     | Internal error | The node failed to compute the hash     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const hash = ethers.keccak256(ethers.toUtf8Bytes('hello world'));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

import { keccak256, toHex } from 'viem';
const hash = keccak256(toHex('hello world'));
```
{% endcode %}
{% endtab %}
{% endtabs %}
