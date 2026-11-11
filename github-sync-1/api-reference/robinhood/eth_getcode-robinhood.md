---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide to using the
  eth_getCode JSON-RPC method in GetBlock.io Web3 documentation.
---

# eth\_getCode - Robinhood

Returns the bytecode at a contract address. Returns `0x` for EOAs (externally-owned accounts) and addresses with no deployed code. Useful for confirming Stock Token contract deployments.

## Parameters

| Parameter | Type            | Required | Description      |
| --------- | --------------- | -------- | ---------------- |
| address   | DATA (20 bytes) | Yes      | Contract address |
| block     | QUANTITY        | TAG      | Yes              |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
        "latest"
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
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
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
                "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b",
                "latest"
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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x608060405234801561001057600080fd5b50600436106100365760003560e01c8063..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type | Description                            |
| ------ | ---- | -------------------------------------- |
| result | DATA | Bytecode at the address. `0x` for EOAs |

## Use Cases

* Distinguishing contracts from EOAs
* Verifying Stock Token contract deployment matches expected bytecode
* On-chain code introspection tools

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
const result = await provider.send('eth_getCode', ["0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "latest"]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_getCode',
    params: ["0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b", "latest"]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endtab %}
{% endtabs %}
