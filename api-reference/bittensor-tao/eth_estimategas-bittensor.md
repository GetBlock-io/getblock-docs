# eth\_estimategas bittensor

{% hint style="info" %}
**Subtensor EVM JSON-RPC method.** Call against a GetBlock endpoint configured for the **EVM interface** (not the Substrate interface). Subtensor EVM uses chain ID **945** (`0x3b1`). Standard Ethereum tooling — ethers.js, viem, Hardhat, Foundry, MetaMask — works unmodified.
{% endhint %}

Estimates gas needed to execute an EVM transaction on Subtensor without submitting it. Returns the minimum gas units the transaction would consume.

## Parameters

| Parameter  | Type   | Required | Description                         |
| ---------- | ------ | -------- | ----------------------------------- |
| callObject | object | Yes      | Same shape as `eth_call` parameters |

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
            "from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a",
            "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12",
            "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
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
            "from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a",
            "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12",
            "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
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
            "from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a",
            "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12",
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
                        "from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a",
                        "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12",
                        "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"
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
    "result": "0xc350"
}
```
{% endcode %}

## Response Parameters

| Field  | Type     | Description               |
| ------ | -------- | ------------------------- |
| result | QUANTITY | Estimated gas units (hex) |

## Use Cases

* Setting gas limit before signing an EVM transaction
* Detecting transactions that would revert (returns error)

## Error Handling

| Status Code | Error Message     | Cause                                                                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found  | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error    | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                                                          |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any EVM method):
const result = await provider.send('eth_estimateGas', [{"from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a", "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]);
console.log(result);

// Most standard methods have typed wrappers:
// provider.getBalance(addr), provider.getBlock(n), provider.getCode(addr), etc.
```
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({
    method: 'eth_estimateGas',
    params: [{"from": "0x7a3F8b9c2d1e4a6f8c5b3d2e7a9c4f1b8e6d5c3a", "to": "0x4a5d8E91234567890abcdef1234567890ABCDEF12", "data": "0xa9059cbb000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa9604500000000000000000000000000000000000000000000000016345785d8a00000"}]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
