---
description: >-
  Example code for the eth_getBalance JSON-RPC method. Complete guide on how to
  use eth_getBalance JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBalance - ARC

This method returns the USDC balance of the given account on Arc, in USDC base units (6 decimals).

{% hint style="warning" %}
**Important**: divide the raw result by 10⁶ (not 10¹⁸) to obtain USDC. This is the most common integration mistake when porting an EVM dApp to Arc.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                                                                            |
| -------------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to check for balance                                                   |
| blockParameter | string | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
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
    "method": "eth_getBalance",
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
    "method": "eth_getBalance",
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
        "method": "eth_getBalance",
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
    "result": "0x5f5e100"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                                       |
| ------ | ------ | ------------------------------------------------------------------------------------------------- |
| result | string | Account balance in USDC base units (hexadecimal). `0x5f5e100` = 100,000,000 base units = 100 USDC |

## Use Cases

* Wallet USDC balance displays
* Pre-flight funds check before submitting payment transactions
* Stablecoin treasury monitoring
* Merchant settlement dashboards

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

const balance = await provider.getBalance('0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0');
// IMPORTANT: Arc uses 6 decimals (USDC), not 18 (ETH).
// formatUnits(value, 6) returns the USDC amount.
console.log('Balance:', ethers.formatUnits(balance, 6), 'USDC');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain, formatUnits } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const balance = await client.getBalance({
    address: '0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0'
});
// Arc uses 6 decimals — pass the decimals explicitly.
console.log('Balance:', formatUnits(balance, 6), 'USDC');
```
{% endtab %}
{% endtabs %}
