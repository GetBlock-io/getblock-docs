---
description: >-
  Example code for the validateaddress JSON-RPC method. Сomplete guide on how to
  use the validateaddress JSON-RPC method in GetBlock.io Web3 documentation.
---

# validateaddress - Zcash

This method validates a **transparent** Zcash address and returns metadata: whether the address is well-formed, whether it's P2PKH (`t1`) or P2SH (`t3`), and the raw script hash. For shielded and unified addresses, use `z_validateaddress` instead.

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| `address` | string | Yes      | Transparent Zcash address to validate |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": [
        "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'validateaddress',
    params: [
        "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
    ],
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
        'method': 'validateaddress',
        'params': [
        "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
    ],
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
            "method": "validateaddress",
            "params": [
        "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
    ],
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
        "isvalid": true,
        "address": "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
        "scriptPubKey": "76a914c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X688ac",
        "isscript": false
    }
}
```

## Response Parameters

| Parameter      | Type    | Description                                                         |
| -------------- | ------- | ------------------------------------------------------------------- |
| `isvalid`      | boolean | `true` if the address is well-formed                                |
| `address`      | string  | The validated address                                               |
| `scriptPubKey` | string  | Hex-encoded scriptPubKey for this address                           |
| `isscript`     | boolean | `true` if the address is a P2SH (t3) address, `false` if P2PKH (t1) |

## Use Cases

* **Address Input Validation**: Validate user-supplied addresses in wallet UIs before allowing send
* **Address Type Detection**: Distinguish P2PKH from P2SH addresses to select the correct signing path
* **Explorer Search Sanity**: Verify search input is a valid address before running expensive queries

## Error Handling

| Error Code | Message        | Description                               |
| ---------- | -------------- | ----------------------------------------- |
| -32602     | Invalid params | Address parameter is missing or malformed |
| -32603     | Internal error | Node failed to validate the address       |
