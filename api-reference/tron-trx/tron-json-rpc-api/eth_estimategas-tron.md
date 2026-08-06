---
description: >-
  Example code for the eth_estimateGas JSON_RPC method. Complete guide on how to
  use eth_estimateGas JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_estimateGas - Tron

This method returns an estimate of the Energy required to execute a transaction, expressed in the Ethereum-compatible gas field. On TRON, this maps to Energy rather than gas.

## Parameters

| Parameter   | Type   | Required | Description             |
| ----------- | ------ | -------- | ----------------------- |
| transaction | object | Yes      | Transaction call object |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0xa9059cbb..."}],
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
    method: 'eth_estimateGas',
    params: [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0xa9059cbb..."}],
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
        'method': 'eth_estimateGas',
        'params': [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0xa9059cbb..."}],
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
            "method": "eth_estimateGas",
            "params": [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0xa9059cbb..."}],
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
    "result": "0xfb1d"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded estimated Energy, expressed as gas |

## Use Cases

* **Fee Preview**: Estimate the Energy cost of a call
* **Fee Limits**: Derive a fee limit from the estimate
* **Revert Detection**: Detect a call that would fail before sending
* **Compatibility**: Provide gas estimates to Ethereum-style tools

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| -32602     | Invalid params  | Invalid transaction object or block parameter            |
| -32000     | Execution error | The call could not be executed against the current state |
| -32603     | Internal error  | The node failed to process the request                   |
