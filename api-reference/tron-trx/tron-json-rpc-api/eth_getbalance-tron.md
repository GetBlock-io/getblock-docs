---
description: >-
  Example code for the eth_getBalance JSON_RPC method. Complete guide on how to
  use eth_getBalance JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_getBalance - Tron

This method returns the TRX balance of an account at a given block, in SUN, hex-encoded. The account address is the TRON address in hex (41-prefixed) form.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | TRON account address in hex (0x41...) form              |
| block     | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": ["0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "latest"],
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
    method: 'eth_getBalance',
    params: ["0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "latest"],
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
        'method': 'eth_getBalance',
        'params': ["0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "latest"],
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
            "method": "eth_getBalance",
            "params": ["0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "latest"],
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
    "result": "0x680a6604"
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | string | Hex-encoded TRX balance in SUN (1 TRX = 1,000,000 SUN) |

## Use Cases

* **Balance Display**: Show an account's TRX balance to Ethereum-style tooling
* **Payment Checks**: Confirm an address holds enough TRX
* **Analytics**: Read balances through a JSON-RPC pipeline
* **Compatibility**: Bridge TRON balances into EVM-oriented tools

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
