---
description: >-
  Example code for the buildTransaction JSON_RPC method. Complete guide on how
  to use buildTransaction JSON_RPC method in GetBlock Web3 documentation.
---

# buildTransaction - Tron

This TRON-specific JSON-RPC method builds an unsigned transaction from an Ethereum-style call object, returning a TRON transaction ready to sign. It bridges Ethereum-style tooling to TRON's native transaction format.

## Parameters

| Parameter   | Type   | Required | Description                                         |
| ----------- | ------ | -------- | --------------------------------------------------- |
| transaction | object | Yes      | Call object with from, to, value, and optional data |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "buildTransaction",
    "params": [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "value": "0x0", "data": "0xa9059cbb..."}],
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
    method: 'buildTransaction',
    params: [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "value": "0x0", "data": "0xa9059cbb..."}],
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
        'method': 'buildTransaction',
        'params': [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "value": "0x0", "data": "0xa9059cbb..."}],
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
            "method": "buildTransaction",
            "params": [{"from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8", "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "value": "0x0", "data": "0xa9059cbb..."}],
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
        "transaction": {
            "txID": "d5ec749e...",
            "raw_data": {
                "contract": [
                    {
                        "type": "TriggerSmartContract"
                    }
                ]
            },
            "raw_data_hex": "0a02...",
            "visible": false
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | object | The built, unsigned TRON transaction ready for signing |

### Result Object

| Field                      | Type   | Description                                    |
| -------------------------- | ------ | ---------------------------------------------- |
| transaction.txID           | string | The transaction id of the unsigned transaction |
| transaction.raw\_data      | object | The TRON contract and metadata                 |
| transaction.raw\_data\_hex | string | Hex serialization used as the signing payload  |

## Use Cases

* **EVM Tooling Bridge**: Build TRON transactions from Ethereum-style call objects
* **Contract Calls**: Prepare a TRC-20 transfer for signing
* **Offline Signing**: Return an unsigned transaction to sign externally
* **Migration**: Ease porting of Ethereum-oriented backends to TRON

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| -32602     | Invalid params  | Invalid transaction object or block parameter            |
| -32000     | Execution error | The call could not be executed against the current state |
| -32603     | Internal error  | The node failed to process the request                   |
