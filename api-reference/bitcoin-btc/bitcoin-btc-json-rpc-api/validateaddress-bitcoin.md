# validateaddress bitcoin

This method returns information about a Bitcoin address, including whether it is valid and its script type. It does not require the address to belong to the wallet.

## Parameters

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| address   | string | Yes      | The Bitcoin address to validate |

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
    "params": ["bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"],
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
    method: 'validateaddress',
    params: ["bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"],
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
        'params': ["bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"],
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
            "params": ["bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq"],
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
        "address": "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq",
        "scriptPubKey": "00141d0f172a0ecb48aee1be1f2687d2963ae33f71a1",
        "isscript": false,
        "iswitness": true,
        "witness_version": 0,
        "witness_program": "1d0f172a0ecb48aee1be1f2687d2963ae33f71a1"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Address validation object               |

### Result Object

| Field        | Type    | Description                                       |
| ------------ | ------- | ------------------------------------------------- |
| isvalid      | boolean | Whether the address is valid                      |
| address      | string  | The validated address                             |
| scriptPubKey | string  | The hex-encoded scriptPubKey for the address      |
| iswitness    | boolean | Whether the address is a witness (SegWit) address |

## Use Cases

* **Input Validation**: Validate addresses before sending
* **Address Typing**: Detect SegWit, Taproot, or legacy addresses
* **Wallet UX**: Reject invalid recipient addresses
* **Compliance**: Normalize and classify addresses

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
