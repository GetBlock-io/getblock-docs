---
description: >-
  Example code for the decodescript JSON-RPC method. Complete guide on how to use
  decodescript JSON-RPC in GetBlock Web3 documentation.
---

# decodescript - Dogecoin

This method decodes a hex-encoded script and returns its assembly form, type, and any addresses it pays to.

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| hexstring | string | Yes      | The hex-encoded script       |

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
    "params": ["76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac"],
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
  jsonrpc: '2.0', method: 'decodescript', params: ["76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac"], id: 'getblock.io'
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'decodescript',
        'params': ["76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac"],
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
            "params": ["76a914986ae75df449ca5e04503c0baa69bbeca93fcf6188ac"],
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
        "asm": "OP_DUP OP_HASH160 986ae75df449ca5e04503c0baa69bbeca93fcf61 OP_EQUALVERIFY OP_CHECKSIG",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
            "DK31KDnpWXT5DyhZgrYY86VdQ92pp7hXmP"
        ],
        "p2sh": "A4yCtkGGYCBLPCLGZpKMvDgvmmpjKjMaJK"
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.asm | string | The script in assembly notation |
| result.reqSigs | numeric | Number of signatures required to spend |
| result.type | string | Script type, such as `pubkeyhash`, `scripthash`, or `multisig` |
| result.addresses | array | Addresses the script pays to |
| result.p2sh | string | The P2SH address that wraps this script |

## Use Cases

* **Script Inspection**: Read what a locking script does before spending it
* **Address Extraction**: Resolve the addresses behind a raw output script
* **Multisig Setup**: Read the P2SH address that wraps a redeem script
* **Debugging**: Confirm a constructed script decodes as intended

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -22 | Argument must be hexadecimal string | The script is not valid hex |
| -32700 | Parse error | Request body is not valid JSON |
| -32603 | Internal error | Node failed to process the request |
