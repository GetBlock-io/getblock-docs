---
description: >-
  Example code for the eth_call JSON_RPC method. Complete guide on how to use
  eth_call JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_call - Tron

This method executes a read-only contract call locally without creating a transaction, in the Ethereum-compatible shape. It is used to read TVM contract state such as TRC-20 balances.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| transaction | object | Yes      | Call object with to and data fields                     |
| block       | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{"to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0x70a08231000000000000000000000000f0cc5a2a84cd0f68ed1667070934542d673acbd8"}, "latest"],
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
    method: 'eth_call',
    params: [{"to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0x70a08231000000000000000000000000f0cc5a2a84cd0f68ed1667070934542d673acbd8"}, "latest"],
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
        'method': 'eth_call',
        'params': [{"to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0x70a08231000000000000000000000000f0cc5a2a84cd0f68ed1667070934542d673acbd8"}, "latest"],
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
            "method": "eth_call",
            "params": [{"to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "data": "0x70a08231000000000000000000000000f0cc5a2a84cd0f68ed1667070934542d673acbd8"}, "latest"],
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
    "result": "0x00000000000000000000000000000000000000000000000000000000000f4240"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded return data from the contract call |

## Use Cases

* **Token Balances**: Query a TRC-20 balance with balanceOf
* **Contract State**: Read view and pure functions from a contract
* **Price Feeds**: Query on-chain oracle values
* **Compatibility**: Read TVM contracts from EVM-oriented tools

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| -32602     | Invalid params  | Invalid transaction object or block parameter            |
| -32000     | Execution error | The call could not be executed against the current state |
| -32603     | Internal error  | The node failed to process the request                   |
