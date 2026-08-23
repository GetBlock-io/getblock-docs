# decodescript bitcoin

This method decodes a hex-encoded script and returns details about it, including its assembly, type, and the addresses it pays to.

## Parameters

| Parameter | Type   | Required | Description    |
| --------- | ------ | -------- | -------------- |
| hexstring | string | Yes      | The script hex |

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
    "params": ["76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac"],
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
    method: 'decodescript',
    params: ["76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac"],
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
        'method': 'decodescript',
        'params': ["76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac"],
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
            "method": "decodescript",
            "params": ["76a914d0f172a0ecb48aee1be1f2687d2963ae33f71a1088ac"],
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
        "asm": "OP_DUP OP_HASH160 d0f172a0ecb48aee1be1f2687d2963ae33f71a10 OP_EQUALVERIFY OP_CHECKSIG",
        "desc": "addr(bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq)#checksum",
        "type": "pubkeyhash",
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "p2sh": "3P14159f73E4gFr7JterCCQh9QjiTjiZrG"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Decoded script object                   |

### Result Object

| Field   | Type   | Description                                                            |
| ------- | ------ | ---------------------------------------------------------------------- |
| asm     | string | Script in assembly notation                                            |
| type    | string | Output type (pubkeyhash, scripthash, witness\_v0\_keyhash, and others) |
| address | string | Address the script pays to, when applicable                            |
| p2sh    | string | The P2SH address wrapping this script                                  |

## Use Cases

* **Script Analysis**: Understand a locking script
* **Address Derivation**: Read the address a script encodes
* **Debugging**: Inspect custom scripts
* **Tooling**: Decode scripts in explorers

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                |
| -22        | Decode failed     | The transaction or script could not be decoded |
| -8         | Invalid parameter | A parameter is out of range or malformed       |
| -32602     | Invalid params    | A parameter is missing or has the wrong type   |
