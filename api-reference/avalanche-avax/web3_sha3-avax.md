---
description: >-
  Example code for the web3_sha3 JSON-RPC method. Complete guide on how to use
  web3_sha3 JSON-RPC in GetBlock Web3 documentation.
---

# web3\_sha3 - AVAX

Returns the Keccak-256 hash of the given input data. Use it to compute event signatures, function selectors, or hashes for off-chain validation. Despite the name `sha3`, the algorithm is Keccak-256, not standard SHA-3.

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| data      | string | Yes      | Hex-encoded data to hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c64"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c64"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c64"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "web3_sha3",
        "params": [
                "0x68656c6c6f20776f726c64"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                 |
| ------ | ------ | ------------------------------------------- |
| result | string | Keccak-256 hash of the input (32 bytes hex) |

## Use Cases

* Computing event topic hashes (e.g. `Transfer(address,address,uint256)`)
* Computing function selectors (first 4 bytes of method signature hash)
* Off-chain hash validation against on-chain data

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('web3_sha3', ["0x68656c6c6f20776f726c64"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const result = await client.request({ method: 'web3_sha3', params: ["0x68656c6c6f20776f726c64"] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
