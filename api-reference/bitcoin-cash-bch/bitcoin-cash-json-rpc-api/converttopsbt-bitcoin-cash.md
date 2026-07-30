---
description: >-
  Example code for the converttopsbt JSON-RPC method. Complete guide on how to
  use converttopsbt JSON-RPC in GetBlock Web3 documentation.
---

# converttopsbt - Bitcoin Cash

This method converts a raw transaction to a PSBT. Any existing signatures are discarded, producing an unsigned PSBT ready for signing.

## Parameters

| Parameter     | Type    | Required | Description                                                                                    |
| ------------- | ------- | -------- | ---------------------------------------------------------------------------------------------- |
| hexstring     | string  | Yes      | The raw transaction hex to convert                                                             |
| permitsigdata | boolean | No       | If true, any signatures in the input are discarded rather than causing an error. Default false |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "converttopsbt",
    "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'converttopsbt',
  params: [rawTxHex, false], id: 'getblock.io'
});
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'converttopsbt',
        'params': ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", false],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "converttopsbt",
            "params": ["01000000000101e17e03d21d051aa2bd9d336c3ac0693cfa92ce71592ceec521b1c48019ff77a101000000171600146d76e574b5f4825fe740ba6c41aaf1b319dfb80cffffffff02819a010000000000160014422002d927a1cae901eac668444cce8dd0ae60d529b31b0b0000000017a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487000000000", false],
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
    "result": "cHNidP8BAHECAAAAAeJ+A9IdBRqivZ0zbDrAaTz6ks5xWSzuxSGxxIAZ/3ehAQAAAAD/////AqGaAQAAAAAAFgAUQiAC2SehyukB6sZoREzOjdCuYNUps7ELAAAAABepFPW0jREw3D02bR6r9ng6VS0cjgj0hwAAAAAAAAAA"
}
```

## Response Parameters

| Parameter | Type         | Description                                            |
| --------- | ------------ | ------------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null       |
| id        | string       | Request identifier matching the request                |
| result    | string       | Base64-encoded PSBT converted from the raw transaction |

## Use Cases

* **Legacy Migration**: Move an existing unsigned transaction into the PSBT workflow
* **Signature Reset**: Strip signatures to re-sign a transaction differently
* **Interop**: Convert externally built transactions for PSBT-based signers
* **Offline Prep**: Prepare a PSBT from a raw template for air-gapped signing

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
