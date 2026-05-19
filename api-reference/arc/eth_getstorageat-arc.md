---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - ARC

This method returns the value stored at a specific storage slot of a contract. Storage slots are 32 bytes; the layout depends on Solidity's storage encoding rules.

## Parameters

| Parameter       | Type   | Required | Description                                                                            |
| --------------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| address         | string | Yes      | 20-byte address of the contract                                                        |
| storagePosition | string | Yes      | Hex-encoded storage slot index (32 bytes)                                              |
| blockParameter  | string | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        "0x0",
        "latest"
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
    "method": "eth_getStorageAt",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        "0x0",
        "latest"
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
    "method": "eth_getStorageAt",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        "0x0",
        "latest"
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
        "method": "eth_getStorageAt",
        "params": [
                "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
                "0x0",
                "latest"
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000539"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                    |
| ------ | ------ | ---------------------------------------------- |
| result | string | Value at the given storage slot (32 bytes hex) |

## Use Cases

* Reading private contract state for debugging
* Inspecting proxy implementation slots (EIP-1967)
* Manual verification of storage layout
* Off-chain emulation and forensics

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
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getStorageAt', ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "0x0", "latest"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    network: 'arc-testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } },
    blockExplorers: { default: { name: 'arcscan', url: 'https://testnet.arcscan.app' } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getStorageAt', params: ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "0x0", "latest"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
