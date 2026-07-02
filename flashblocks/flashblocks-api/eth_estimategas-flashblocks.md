---
description: >-
  Example code for the eth_estimateGas - Flashblocks method. Complete guide on
  how to use eth_estimateGas - Flashblocks in GetBlock Web3 documentation.
---

# eth\_estimateGas - Flashblocks

This estimates the gas needed to execute a transaction without submitting it. When called with `"pending"`, the estimate is computed against state including preconfirmed transactions in the latest Flashblock — giving more accurate estimates for transactions that depend on state changes happening in the current block.

## Parameters

| Parameter  | Type     | Required | Description                                                                     |
| ---------- | -------- | -------- | ------------------------------------------------------------------------------- |
| callObject | object   | Yes      | Same shape as `eth_call` parameters                                             |
| block      | QUANTITY | No       | `"pending"` for gas estimate against preconfirmed state (default is `"latest"`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"
        },
        "pending"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"
        },
        "pending"
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
{% endcode %}
{% endtab %}

{% tab title="Python (Requests)" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
            "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
            "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"
        },
        "pending"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust (Reqwest)" %}
{% code overflow="wrap" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_estimateGas",
        "params": [
                {
                        "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
                        "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
                        "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"
                },
                "pending"
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
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xc350"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description               |
| ------ | -------- | ------------------------- |
| result | QUANTITY | Estimated gas units (hex) |

## Use Cases

* Accurate gas estimation for transactions that depend on preconfirmed state (e.g. sequential swaps in a bundle)
* Detecting transactions that would revert given preconfirmed state changes
* Wallet fee suggestion UIs with Flashblocks-aware accuracy

## Error Handling

| Status Code | Error Message      | Cause                                                                                |
| ----------- | ------------------ | ------------------------------------------------------------------------------------ |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                                                  |
| -32602      | Invalid params     | Request parameters are missing or malformed                                          |
| -32603      | Internal error     | Server-side error while processing the request                                       |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                                                    |
| 3           | execution reverted | Transaction would revert against the preconfirmed state — estimation cannot complete |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call — 'pending' returns Flashblocks-preconfirmed state:
const result = await provider.send('eth_estimateGas', [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"}, "pending"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider that accept 'pending':
// provider.getBalance(addr, 'pending'), provider.getTransactionCount(addr, 'pending'), etc.
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// viem's read methods accept blockTag: 'pending' for Flashblocks-preconfirmed state:
const result = await client.request({
    method: 'eth_estimateGas',
    params: [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913", "data": "0xa9059cbb0000000000000000000000007a25d0f0ff6a25f36de0a19c3d5f6dc2c6eaed6a00000000000000000000000000000000000000000000000000000000000186a0"}, "pending"]
});
console.log(result);

// Typed wrappers also accept blockTag: 'pending':
// client.getBalance({ address, blockTag: 'pending' })
// client.getBlock({ blockTag: 'pending' })
```
{% endcode %}
{% endtab %}
{% endtabs %}
