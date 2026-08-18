---
description: >-
  Example code for the decodescript JSON-RPC method. Complete guide on how to
  use decodescript JSON-RPC in GetBlock Web3 documentation.
---

# decodescript - Bitcoin Cash

This method decodes a hex-encoded script into a JSON object describing its assembly, type, and any associated addresses.

## Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| hexstring | string | Yes      | The hex-encoded script to decode |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487"],
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
  jsonrpc: '2.0', method: 'decodescript',
  params: ['a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487'], id: 'getblock.io'
});
console.log(data.result.type, data.result.addresses);
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
        'method': 'decodescript',
        'params': ["a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487"],
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
            "method": "decodescript",
            "params": ["a914f5b48d1130dc3d366d1eabf6783a552d1c8e08f487"],
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

{% code overflow="wrap" %}
```json
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "asm": "OP_HASH160 f5b48d1130dc3d366d1eabf6783a552d1c8e08f4 OP_EQUAL",
        "type": "scripthash",
        "reqSigs": 1,
        "addresses": [
            "bitcoincash:ppm2qsznhks23z7629mms6s4cwef74vcwvn0h829pq"
        ],
        "p2sh": "bitcoincash:ppm2qsznhks23z7629mms6s4cwef74vcwvn0h829pq"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Decoded script object                            |

### Result Object

| Field     | Type    | Description                                   |
| --------- | ------- | --------------------------------------------- |
| asm       | string  | Script in assembly notation                   |
| type      | string  | Script type, such as pubkeyhash or scripthash |
| reqSigs   | numeric | Number of required signatures                 |
| addresses | array   | Addresses associated with the script          |

## Use Cases

* **Script Analysis**: Understand the spending conditions of an output script
* **Address Derivation**: Compute the P2SH address for a redeem script
* **Contract Inspection**: Decode custom scripts when building applications
* **Debugging**: Verify a constructed script assembles as intended

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
