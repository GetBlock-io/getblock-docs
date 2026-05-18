---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Сomplete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - opBNB

This method returns information about a block by its hash. Like `eth_getBlockByNumber`, the second argument controls whether transactions are returned as full objects or only hashes.

## Parameters

| Parameter        | Type    | Required | Description                                                          |
| ---------------- | ------- | -------- | -------------------------------------------------------------------- |
| blockHash        | string  | Yes      | 32-byte hash of the block                                            |
| fullTransactions | boolean | Yes      | If `true`, returns full transaction objects; if `false`, only hashes |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        false
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
    "method": "eth_getBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        false
    ],
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        false
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
        "method": "eth_getBlockByHash",
        "params": [
                "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
                false
        ],
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "number": "0x2d4e8a3",
        "hash": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        "parentHash": "0x3a2e9d4b1c5f8a7e6d4b2c1f9a3e5d8b7c2f4a1d6e9b3c5f8a2d4e7b1c9f3a6e",
        "timestamp": "0x67891f24",
        "gasLimit": "0x5f5e100",
        "gasUsed": "0x2faf080",
        "transactions": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type           | Description                                                                  |
| ------------------- | -------------- | ---------------------------------------------------------------------------- |
| result              | object \| null | Block object (same schema as `eth_getBlockByNumber`), or `null` if not found |
| result.number       | string         | Block number (hex)                                                           |
| result.hash         | string         | Block hash (32 bytes hex)                                                    |
| result.timestamp    | string         | Unix timestamp (hex)                                                         |
| result.transactions | array          | Array of transaction hashes or full objects                                  |

## Use Cases

* Resolving a block from a known hash (e.g. from a transaction receipt)
* Reorg detection by comparing block hashes at the same height
* Verifying block identity for trust-minimised applications

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getBlockByHash', ["0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b", false]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';
import { opBNB } from 'viem/chains';

const client = createPublicClient({
    chain: opBNB,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getBlockByHash', params: ["0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b", false] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
