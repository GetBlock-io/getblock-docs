---
description: >-
  Example code for the decodepsbt JSON-RPC method. Complete guide on how to use
  decodepsbt JSON-RPC in GetBlock Web3 documentation.
---

# decodepsbt - Bitcoin Cash

This method decodes a base64-encoded partially signed transaction into a JSON object describing its inputs, outputs, and any partial signatures.

## Parameters

<table><thead><tr><th width="189.20489501953125">Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td>psbt</td><td>string</td><td>Yes</td><td>The base64-encoded PSBT to decode</td></tr></tbody></table>

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
    "params": ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'decodepsbt',
  params: [psbt], id: 'getblock.io'
});
console.log(data.result.tx.txid);
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
        'params': ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
            "params": ["cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"],
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
    "error": null,
    "id": "getblock.io",
    "result": {
        "tx": {
            "txid": "10b54fd708ab2e5703979b4ba27ca0339882abc2062e77fbe51e625203a49642",
            "version": 2,
            "locktime": 0,
            "vin": [
                {
                    "txid": "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587",
                    "vout": 1,
                    "sequence": 4294967295
                }
            ],
            "vout": [
                {
                    "value": 0.001,
                    "n": 0,
                    "scriptPubKey": {
                        "type": "pubkeyhash",
                        "addresses": [
                            "bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a"
                        ]
                    }
                }
            ]
        },
        "inputs": [
            {
                "witness_utxo": {
                    "amount": 0.00105345
                }
            }
        ],
        "outputs": [
            {}
        ],
        "fee": 2.26e-06
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                                    |
| --------- | ------------ | -------------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null               |
| id        | string       | Request identifier matching the request                        |
| result    | object       | Decoded PSBT object with transaction, inputs, outputs, and fee |

### Result Object

| Field   | Type    | Description                                                |
| ------- | ------- | ---------------------------------------------------------- |
| tx      | object  | The decoded unsigned transaction                           |
| inputs  | array   | Per-input PSBT data including UTXOs and partial signatures |
| outputs | array   | Per-output PSBT data                                       |
| fee     | numeric | Transaction fee in BCH, when computable                    |

## Use Cases

* **Signing Review**: Inspect a PSBT's contents before adding a signature
* **Fee Verification**: Read the computed fee before finalizing a PSBT
* **Multisig Coordination**: Check which inputs still need signatures
* **Debugging**: Diagnose a PSBT that fails to finalize

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
