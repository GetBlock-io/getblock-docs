---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide to using
  the eth_estimateGas JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_estimateGas - Robinhood

Estimates the gas needed to execute a transaction without submitting it. Returns the minimum gas units the transaction would consume.

## Parameters

* `callObject` (`object`, required): Same shape as `eth_call` parameters
* `block` (`QUANTITY` / `TAG`, optional): Block reference for the estimation context (defaults to `latest`)

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
        }
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
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
        }
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a",
            "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
            "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
        }
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
                        "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                        "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
                }
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xc350"
}
```

## Response Parameters

| Field  | Type     | Description               |
| ------ | -------- | ------------------------- |
| result | QUANTITY | Estimated gas units (hex) |

## Use Cases

* Setting gas limit before signing a transaction
* Detecting transactions that would revert (returns error)
* Cost estimation in wallet UIs

## Error Handling

| Status Code | Error Message      | Cause                                                 |
| ----------- | ------------------ | ----------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                   |
| -32602      | Invalid params     | Request parameters are missing or malformed           |
| -32603      | Internal error     | Server-side error while processing the request        |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                     |
| 3           | execution reverted | Transaction would revert — estimation cannot complete |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_estimateGas', [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_estimateGas',
    params: [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endcode %}
{% endtab %}
{% endtabs %}
