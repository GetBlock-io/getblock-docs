---
description: >-
  Example code for the finalizepsbt JSON-RPC method. Complete guide on how to
  use finalizepsbt JSON-RPC in GetBlock Web3 documentation.
---

# finalizepsbt - Bitcoin

This method finalizes a PSBT once all inputs are signed and, if complete, extracts the network-ready transaction. It returns the finalized PSBT or the extracted hex.

## Parameters

| Parameter | Type    | Required | Description                                                                   |
| --------- | ------- | -------- | ----------------------------------------------------------------------------- |
| psbt      | string  | Yes      | The base64-encoded PSBT                                                       |
| extract   | boolean | No       | Whether to extract and return the raw transaction if complete (default: true) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "finalizepsbt",
    "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="],
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
    method: 'finalizepsbt',
    params: ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="],
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
        'method': 'finalizepsbt',
        'params': ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="],
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
            "method": "finalizepsbt",
            "params": ["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="],
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
        "hex": "0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000",
        "complete": true
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                       |
| id        | string | Request identifier matching the request                 |
| result    | object | Object with the finalized PSBT or extracted transaction |

### Result Object

| Field    | Type    | Description                                            |
| -------- | ------- | ------------------------------------------------------ |
| psbt     | string  | The finalized PSBT, when not extracting                |
| hex      | string  | The extracted network-ready transaction, when complete |
| complete | boolean | Whether the transaction is fully signed and finalized  |

## Use Cases

* **Transaction Extraction**: Produce a broadcastable transaction from a PSBT
* **Signing Completion**: Finalize once all signatures are present
* **Custody**: Extract from a fully signed PSBT
* **Automation**: Finalize and broadcast in a pipeline

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                |
| -22        | Decode failed     | The transaction or script could not be decoded |
| -8         | Invalid parameter | A parameter is out of range or malformed       |
| -32602     | Invalid params    | A parameter is missing or has the wrong type   |
