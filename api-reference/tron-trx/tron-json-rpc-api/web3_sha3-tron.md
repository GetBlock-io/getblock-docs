---
description: >-
  Example code for the web3_sha3 JSON_RPC method. Complete guide on how to use
  web3_sha3 JSON_RPC method in GetBlock Web3 documentation.
---

# web3\_sha3 - Tron

This method returns the Keccak-256 hash of the given data. It is used to compute function selectors and event topic hashes.

## Parameters

| Parameter | Type   | Required | Description                   |
| --------- | ------ | -------- | ----------------------------- |
| data      | string | Yes      | The data to hash, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": ["0x68656c6c6f20776f726c64"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc', {
    jsonrpc: '2.0',
    method: 'web3_sha3',
    params: ["0x68656c6c6f20776f726c64"],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'web3_sha3',
        'params': ["0x68656c6c6f20776f726c64"],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "web3_sha3",
            "params": ["0x68656c6c6f20776f726c64"],
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
    "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Keccak-256 hash of the input data       |

## Use Cases

* **Function Selectors**: Compute the 4-byte selector for a function signature
* **Event Topics**: Compute the topic hash for an event signature
* **Data Integrity**: Hash data for verification
* **Tooling**: Provide Keccak-256 hashing to Ethereum-style tools

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
