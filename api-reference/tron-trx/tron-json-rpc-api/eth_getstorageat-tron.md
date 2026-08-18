---
description: >-
  Example code for the eth_getStorageAt JSON_RPC method. Complete guide on how
  to use eth_getStorageAt JSON_RPC method in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Tron

This method returns the value stored at a given storage slot of a contract, hex-encoded.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| address   | string | Yes      | Contract address in hex (0x41...) form                  |
| position  | string | Yes      | Hex-encoded storage slot index                          |
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
    "method": "eth_getStorageAt",
    "params": ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "0x0", "latest"],
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
    method: 'eth_getStorageAt',
    params: ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "0x0", "latest"],
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
        'method': 'eth_getStorageAt',
        'params': ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "0x0", "latest"],
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
            "method": "eth_getStorageAt",
            "params": ["0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c", "0x0", "latest"],
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")             |
| id        | string | Request identifier matching the request       |
| result    | string | 32-byte hex-encoded value at the storage slot |

## Use Cases

* **Raw Storage Reads**: Read state not exposed by a getter
* **Proxy Slots**: Read an implementation address from a proxy slot
* **Debugging**: Inspect a contract's raw storage layout
* **Verification**: Confirm a storage value against expected state

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
