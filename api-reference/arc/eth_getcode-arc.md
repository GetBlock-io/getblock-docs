---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - ARC

This method returns the contract bytecode deployed at a given address. An empty result (`0x`) indicates that the address is an externally owned account (EOA), not a contract.

## Parameters

| Parameter      | Type   | Required | Description                                                                            |
| -------------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| address        | string | Yes      | 20-byte address of the contract                                                        |
| blockParameter | string | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
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
    "method": "eth_getCode",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
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
    "method": "eth_getCode",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
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
        "method": "eth_getCode",
        "params": [
                "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
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
    "result": "0x6080604052348015600f57600080fd5b5060..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                |
| ------ | ------ | ---------------------------------------------------------- |
| result | string | Contract bytecode in hex, or `0x` if the address is an EOA |

## Use Cases

* Distinguishing contracts from EOAs
* Verifying contract deployment on Arc
* Comparing on-chain bytecode against expected build output
* Detecting ERC-4337 smart accounts and EIP-7702 delegated EOAs

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
const result = await provider.send('eth_getCode', ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "latest"]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
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

const result = await client.request({ method: 'eth_getCode', params: ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", "latest"] });
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
