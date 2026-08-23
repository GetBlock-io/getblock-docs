---
description: >-
  Example code for the deriveaddresses JSON-RPC method. Complete guide on how to
  use deriveaddresses JSON-RPC in GetBlock Web3 documentation.
---

# deriveaddresses - Bitcoin

This method derives one or more addresses from an output descriptor. A range is required for descriptors that contain a wildcard.

## Parameters

| Parameter  | Type            | Required | Description                                           |
| ---------- | --------------- | -------- | ----------------------------------------------------- |
| descriptor | string          | Yes      | The output descriptor                                 |
| range      | number or array | No       | The range of indices to derive for ranged descriptors |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "deriveaddresses",
    "params": ["wpkh([d34db33f/84h/0h/0h]xpub.../0/*)#checksum", [0, 2]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'deriveaddresses',
    params: ["wpkh([d34db33f/84h/0h/0h]xpub.../0/*)#checksum", [0, 2]],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'deriveaddresses',
        'params': ["wpkh([d34db33f/84h/0h/0h]xpub.../0/*)#checksum", [0, 2]],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "deriveaddresses",
            "params": ["wpkh([d34db33f/84h/0h/0h]xpub.../0/*)#checksum", [0, 2]],
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
    "result": [
        "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
        "bc1qw508d6qejxtdg4y5r3zarvary0c5xw7kv8f3t4"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | array  | Array of derived addresses              |

## Use Cases

* **Descriptor Wallets**: Derive addresses from a descriptor
* **Watch-Only Setups**: Generate receive addresses off-node
* **Address Gap Scanning**: Derive a range for scanning
* **Auditing**: Reproduce a wallet's addresses

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
