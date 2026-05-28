---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide on how to
  use eth_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - AVAX

Returns the AVAX balance of an account on the C-Chain, in wei. Divide by 10¹⁸ to obtain AVAX.

{% hint style="info" %}
This returns the C-Chain balance only. For P-Chain or X-Chain balances, use `platform.getBalance` or `avm.getBalance`.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                          |
| -------------- | ------ | -------- | ---------------------------------------------------- |
| address        | string | Yes      | 20-byte C-Chain address (0x...) to check for balance |
| blockParameter | string | Yes      | Block number in hex, or block tag                    |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
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
    "method": "eth_getBalance",
    "params": [
        "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
        "latest"
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
    "method": "eth_getBalance",
    "params": [
        "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
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
        "method": "eth_getBalance",
        "params": [
                "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
                "latest"
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
    "result": "0x1bc16d674ec80000"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                          |
| ------ | ------ | ---------------------------------------------------- |
| result | string | Account AVAX balance on C-Chain in wei (hexadecimal) |

## Use Cases

* Wallet balance displays for AVAX on C-Chain
* Pre-flight funds check before submitting transactions
* DeFi protocol balance monitoring

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc');

const balance = await provider.getBalance('0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2');
console.log('Balance:', ethers.formatEther(balance), 'AVAX');
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, formatEther } from 'viem';
import { avalanche } from 'viem/chains';

const client = createPublicClient({
    chain: avalanche,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc')
});

const balance = await client.getBalance({ address: '0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2' });
console.log('Balance:', formatEther(balance), 'AVAX');
```
{% endtab %}
{% endtabs %}
