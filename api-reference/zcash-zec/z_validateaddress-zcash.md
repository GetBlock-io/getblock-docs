---
description: >-
  Example code for the z_validateaddress JSON-RPC method. Сomplete guide on how
  to use the z_validateaddress JSON-RPC method in GetBlock.io Web3
  documentation.
---

# z\_validateaddress - Zcash

This method validates a Zcash shielded or unified address (Sapling `zs`, Orchard, or Unified `u`) and returns type-specific metadata. Unified addresses can contain multiple receivers (transparent + Sapling + Orchard); the response indicates which types are present. Use `validateaddress` for transparent addresses.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| `address` | string | Yes      | Shielded or Unified Zcash address |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "z_validateaddress",
    "params": [
        "u1l8xunezsvhq8fgzfl7404m450nwnd76zshscn6nfys7vyz2ywyh4cc5daaq0c7q2su5lqfh23sp7fkf3kt27ve5948mzpfdvckzaext2jkbmhth7pnfhr37a3wpk3q6clhx"
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
    method: 'z_validateaddress',
    params: [
        "u1l8xunezsvhq8fgzfl7404m450nwnd76zshscn6nfys7vyz2ywyh4cc5daaq0c7q2su5lqfh23sp7fkf3kt27ve5948mzpfdvckzaext2jkbmhth7pnfhr37a3wpk3q6clhx"
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
        'method': 'z_validateaddress',
        'params': [
        "u1l8xunezsvhq8fgzfl7404m450nwnd76zshscn6nfys7vyz2ywyh4cc5daaq0c7q2su5lqfh23sp7fkf3kt27ve5948mzpfdvckzaext2jkbmhth7pnfhr37a3wpk3q6clhx"
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
            "method": "z_validateaddress",
            "params": [
        "u1l8xunezsvhq8fgzfl7404m450nwnd76zshscn6nfys7vyz2ywyh4cc5daaq0c7q2su5lqfh23sp7fkf3kt27ve5948mzpfdvckzaext2jkbmhth7pnfhr37a3wpk3q6clhx"
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "isvalid": true,
        "address_type": "unified",
        "address": "u1l8xunezsvhq8fgzfl7404m450nwnd76zshscn6nfys7vyz2ywyh4cc5daaq0c7q2su5lqfh23sp7fkf3kt27ve5948mzpfdvckzaext2jkbmhth7pnfhr37a3wpk3q6clhx",
        "receiver_types": [
            "orchard",
            "sapling",
            "p2pkh"
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Parameter        | Type            | Description                                                                                    |
| ---------------- | --------------- | ---------------------------------------------------------------------------------------------- |
| `isvalid`        | boolean         | `true` if the address is well-formed                                                           |
| `address_type`   | string          | Address type: `sapling`, `orchard`, `unified`, or `p2pkh`/`p2sh` for transparent               |
| `address`        | string          | The validated address                                                                          |
| `receiver_types` | array of string | For unified addresses: list of receiver types included (`orchard`, `sapling`, `p2pkh`, `p2sh`) |

## Use Cases

* **Shielded Wallet Input Validation**: Verify shielded and unified addresses before allowing send
* **Unified Address Introspection**: Determine which receiver types a unified address supports to select the appropriate spending pool
* **Address Migration Support**: Detect legacy Sapling addresses vs modern Orchard/unified addresses in wallet UX
* **Compliance Address Type Reporting**: Distinguish shielded vs transparent activity for jurisdictional reporting

## Error Handling

| Error Code | Message         | Description                                              |
| ---------- | --------------- | -------------------------------------------------------- |
| -8         | Invalid address | Address is not a valid shielded or unified Zcash address |
| -32602     | Invalid params  | Address parameter is missing or malformed                |
| -32603     | Internal error  | Node failed to validate the address                      |
