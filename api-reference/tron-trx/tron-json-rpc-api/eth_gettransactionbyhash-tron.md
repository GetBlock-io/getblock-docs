---
description: >-
  Example code for the eth_getTransactionByHash JSON_RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON_RPC method in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Tron

This method returns information about a transaction by its hash, in the Ethereum-compatible shape.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc', {
    jsonrpc: '2.0',
    method: 'eth_getTransactionByHash',
    params: ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getTransactionByHash',
        'params': ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getTransactionByHash",
            "params": ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
        "blockHash": "0x0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
        "blockNumber": "0x40d2a00",
        "from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8",
        "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "value": "0x0",
        "input": "0xa9059cbb...",
        "transactionIndex": "0x0"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| id        | string | Request identifier matching the request  |
| result    | object | Transaction object, or null if not found |

### Result Object

| Field       | Type   | Description                                   |
| ----------- | ------ | --------------------------------------------- |
| hash        | string | Transaction hash                              |
| blockNumber | string | Block number of inclusion, or null if pending |
| from        | string | Sender address in hex form                    |
| to          | string | Recipient or contract address in hex form     |
| input       | string | Call data, for contract transactions          |

## Use Cases

* **Transaction Views**: Display a transaction in Ethereum-style tooling
* **Status Tracking**: Check whether a transaction is included
* **Call Data Reads**: Read the input data of a contract call
* **Compatibility**: Feed TRON transactions into EVM-oriented tools

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
