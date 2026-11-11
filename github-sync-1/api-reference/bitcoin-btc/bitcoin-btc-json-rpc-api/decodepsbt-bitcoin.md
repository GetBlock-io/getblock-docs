---
description: >-
  Example code for the decodepsbt JSON-RPC method. Complete guide on how to use
  decodepsbt JSON-RPC in GetBlock Web3 documentation.
---

# decodepsbt - Bitcoin

This method decodes a base64-encoded PSBT into a detailed JSON object, including the unsigned transaction, per-input data, and per-output data.

## Parameters

| Parameter | Type   | Required | Description             |
| --------- | ------ | -------- | ----------------------- |
| psbt      | string | Yes      | The base64-encoded PSBT |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decodepsbt",
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
    method: 'decodepsbt',
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
        'method': 'decodepsbt',
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
            "method": "decodepsbt",
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
        "tx": {
            "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
            "version": 2,
            "locktime": 0,
            "vin": [],
            "vout": []
        },
        "inputs": [
            {
                "witness_utxo": {
                    "amount": 6.25,
                    "scriptPubKey": {
                        "type": "witness_v0_keyhash",
                        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
                    }
                }
            }
        ],
        "outputs": [
            {}
        ],
        "fee": 1e-05
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Decoded PSBT object                     |

### Result Object

| Field   | Type   | Description                                      |
| ------- | ------ | ------------------------------------------------ |
| tx      | object | The decoded unsigned transaction                 |
| inputs  | array  | Per-input PSBT data (UTXOs, signatures, scripts) |
| outputs | array  | Per-output PSBT data                             |
| fee     | number | The transaction fee in BTC, if calculable        |

## Use Cases

* **PSBT Inspection**: Read a PSBT's inputs, outputs, and fee
* **Verification**: Confirm a PSBT before signing
* **Debugging**: Inspect signing progress
* **Tooling**: Render PSBTs in wallet UIs

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                |
| -22        | Decode failed     | The transaction or script could not be decoded |
| -8         | Invalid parameter | A parameter is out of range or malformed       |
| -32602     | Invalid params    | A parameter is missing or has the wrong type   |
