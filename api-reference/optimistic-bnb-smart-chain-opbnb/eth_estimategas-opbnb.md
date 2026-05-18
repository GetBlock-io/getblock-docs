---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Сomplete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock.io Web3 documentation.
---

# eth\_estimateGas - opBNB

This method estimates the gas required to execute a transaction without committing it. Use it to determine an appropriate `gas` value before broadcasting.

## Parameters

| Parameter      | Type   | Required | Description                                                               |
| -------------- | ------ | -------- | ------------------------------------------------------------------------- |
| callObject     | object | Yes      | Transaction-shaped object with `to`, `data`, and optional `from`, `value` |
| blockParameter | string | No       | Optional block parameter (default: `"latest"`)                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0x16345785d8a0000"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0x16345785d8a0000"
        }
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
            "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
            "value": "0x16345785d8a0000"
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
        "method": "eth_estimateGas",
        "params": [
                {
                        "from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2",
                        "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
                        "value": "0x16345785d8a0000"
                }
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
    "result": "0x5208"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                        |
| ------ | ------ | ---------------------------------- |
| result | string | Estimated gas units required (hex) |

## Use Cases

* Setting an appropriate gas limit before signing a transaction
* Showing users an estimated cost in wallet UIs
* Detecting transactions that would revert (estimate fails)
* Computing fee budgets in server-side automation

## Error Handling

| Status Code | Error Message      | Cause                                                 |
| ----------- | ------------------ | ----------------------------------------------------- |
| 403         | Forbidden          | Missing or invalid `<ACCESS-TOKEN>`                   |
| -32602      | Invalid params     | Request parameters are missing or malformed           |
| -32601      | Method not found   | The method is not supported by this node              |
| 429         | Too Many Requests  | Rate limit exceeded for your plan                     |
| -32000      | Execution reverted | Transaction would revert; estimate cannot be returned |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_estimateGas', [{"from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2", "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "value": "0x16345785d8a0000"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { opBNB } from 'viem/chains';

const client = createPublicClient({
    chain: opBNB,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_estimateGas', params: [{"from": "0x9e7c5e3e3a3b8e1aa0e2d4c7f9d4b0c8b8d5f1a2", "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245", "value": "0x16345785d8a0000"}] });
console.log(result);
```
{% endtab %}
{% endtabs %}
