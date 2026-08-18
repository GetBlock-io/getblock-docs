---
description: >-
  Example code for the z_listunifiedreceivers JSON-RPC method. Сomplete guide on
  how to use the z_listunifiedreceivers JSON-RPC method in GetBlock.io Web3
  documentation.
---

# z\_listunifiedreceivers - Zcash

This method decomposes a Unified Address into its component receivers. Unified Addresses (introduced with NU5) can contain a transparent receiver (`p2pkh` or `p2sh`), a Sapling receiver, and/or an Orchard receiver — all in a single encoded string. This method returns the individual receiver strings that a sender can use to direct payments to specific pools.

## Parameters

| Parameter         | Type   | Required | Description                  |
| ----------------- | ------ | -------- | ---------------------------- |
| `unified_address` | string | Yes      | Unified Address to decompose |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "z_listunifiedreceivers",
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
    method: 'z_listunifiedreceivers',
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
        'method': 'z_listunifiedreceivers',
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
            "method": "z_listunifiedreceivers",
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
        "orchard": "u1...orchard-only-receiver...",
        "sapling": "zs1z7rejlpsa98s2rrrfkwmaxu53e4ue0ulcrw0h4x5g8jl04tak0d3mm47vdtahatqrlkngh9sly",
        "p2pkh": "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                                                                      |
| --------- | ------ | ------------------------------------------------------------------------------------------------ |
| `orchard` | string | Orchard receiver as a standalone address string (present if the UA includes an Orchard receiver) |
| `sapling` | string | Sapling receiver as a `zs1...` address (present if the UA includes a Sapling receiver)           |
| `p2pkh`   | string | Transparent P2PKH receiver as a `t1...` address (present if the UA includes a P2PKH receiver)    |
| `p2sh`    | string | Transparent P2SH receiver as a `t3...` address (present if the UA includes a P2SH receiver)      |

## Use Cases

* **Send Path Selection**: Determine which pool(s) a Unified Address supports before selecting a spending strategy
* **Wallet Compatibility Detection**: Sending wallets without Orchard support extract the Sapling receiver as a fallback
* **Address Analytics**: Study Unified Address adoption patterns by receiver-type composition
* **Legacy Fallback Support**: Wallets on older networks fall back to transparent receivers when shielded support is unavailable

## Error Handling

| Error Code | Message               | Description                               |
| ---------- | --------------------- | ----------------------------------------- |
| -8         | Not a Unified Address | Input is not a valid Unified Address      |
| -32602     | Invalid params        | Address parameter is missing or malformed |
| -32603     | Internal error        | Node failed to decompose the address      |
