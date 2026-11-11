---
description: >-
  Example code for the eth_createAccessList JSON-RPC method. Complete guide to
  using the eth_createAccessList JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_createAccessList  - Robinhood

Creates an EIP-2930 access list for a transaction — a list of addresses and storage slots that the transaction expects to access. Pre-warming this list reduces gas costs for transactions touching many slots.

## Parameters

| Parameter  | Type            | Required | Description                            |
| ---------- | --------------- | -------- | -------------------------------------- |
| callObject | object          | Yes      | Same shape as `eth_call` parameters    |
| block      | QUANTITY \| TAG | No       | Block reference (defaults to `latest`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_createAccessList",
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
    "method": "eth_createAccessList",
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
    "method": "eth_createAccessList",
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
        "method": "eth_createAccessList",
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
    "result": {
        "accessList": [
            {
                "address": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                "storageKeys": [
                    "0x000000000000000000000000000000000000000000000000000000000000002a",
                    "0x000000000000000000000000000000000000000000000000000000000000002b"
                ]
            }
        ],
        "gasUsed": "0xc350"
    }
}
```

## Response Parameters

| Field             | Type     | Description                                                       |
| ----------------- | -------- | ----------------------------------------------------------------- |
| result.accessList | array    | EIP-2930 access list — addresses and the storage keys they access |
| result.gasUsed    | QUANTITY | Gas that would be used with this access list                      |

## Use Cases

* EIP-2930 type-1 transactions for predictable gas cost
* Pre-warming state access in complex DeFi transactions
* Audit tooling that analyzes state access patterns

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_createAccessList', [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]);
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
    method: 'eth_createAccessList',
    params: [{"from": "0x7a25d0f0Ff6a25f36De0a19C3d5f6dC2C6EAeD6a", "to": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endcode %}
{% endtab %}
{% endtabs %}
