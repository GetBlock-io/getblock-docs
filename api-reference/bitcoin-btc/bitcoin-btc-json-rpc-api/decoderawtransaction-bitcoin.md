---
description: >-
  Example code for the decoderawtransaction JSON-RPC method. Complete guide on
  how to use decoderawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# decoderawtransaction - Bitcoin

This method decodes a serialized, hex-encoded transaction into a JSON object. It does not require the transaction to be in the blockchain or mempool.

## Parameters

| Parameter | Type    | Required | Description                                                     |
| --------- | ------- | -------- | --------------------------------------------------------------- |
| hexstring | string  | Yes      | The transaction hex                                             |
| iswitness | boolean | No       | Whether the transaction hex is a serialized witness transaction |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
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
    method: 'decoderawtransaction',
    params: ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
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
        'method': 'decoderawtransaction',
        'params': ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
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
            "method": "decoderawtransaction",
            "params": ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
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
        "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "hash": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
        "version": 2,
        "size": 226,
        "vsize": 144,
        "weight": 573,
        "locktime": 0,
        "vin": [
            {
                "txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b",
                "vout": 0,
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "value": 6.24,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP ...",
                    "hex": "76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac",
                    "type": "pubkeyhash",
                    "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"
                }
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Decoded transaction object              |

### Result Object

| Field    | Type   | Description              |
| -------- | ------ | ------------------------ |
| txid     | string | The transaction ID       |
| vin      | array  | Transaction inputs       |
| vout     | array  | Transaction outputs      |
| locktime | number | The transaction locktime |

## Use Cases

* **Transaction Inspection**: Decode a transaction before broadcasting
* **Verification**: Confirm outputs match intent
* **Debugging**: Inspect a constructed transaction
* **Tooling**: Decode transactions in wallet flows

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                |
| -22        | Decode failed     | The transaction or script could not be decoded |
| -8         | Invalid parameter | A parameter is out of range or malformed       |
| -32602     | Invalid params    | A parameter is missing or has the wrong type   |
