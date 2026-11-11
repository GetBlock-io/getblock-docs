---
description: >-
  Example code for the eth_getCode JSON_RPC method. Complete guide on how to use
  eth_getCode JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_getCode - Tron

This method returns the deployed bytecode at an address. An account with no contract returns 0x, while a TVM contract returns its runtime bytecode.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | Contract address in hex (0x41...) form                  |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "latest"],
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
    method: 'eth_getCode',
    params: ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "latest"],
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
        'method': 'eth_getCode',
        'params': ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "latest"],
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
            "method": "eth_getCode",
            "params": ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "latest"],
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
    "result": "0x60806040526004361061..."
}
```

## Response Parameters

| Parameter | Type   | Description                                                    |
| --------- | ------ | -------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                              |
| id        | string | Request identifier matching the request                        |
| result    | string | Hex-encoded runtime bytecode, or 0x for a non-contract account |

## Use Cases

* **Contract Detection**: Distinguish a contract from a plain account
* **Bytecode Verification**: Compare deployed bytecode against an artifact
* **Security Analysis**: Retrieve bytecode for static analysis
* **Compatibility**: Read TVM bytecode from EVM-oriented tools

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
