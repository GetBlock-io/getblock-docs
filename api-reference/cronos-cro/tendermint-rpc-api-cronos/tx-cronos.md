# tx - Cronos

Returns the execution result of a transaction by its hash, including its height, gas, ABCI code, and events, optionally with a Merkle proof.

## Parameters

| Parameter | Type    | Required | Description                               |
| --------- | ------- | -------- | ----------------------------------------- |
| hash      | string  | Yes      | Transaction hash (hex)                    |
| prove     | boolean | Optional | Include a Merkle proof of the transaction |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "tx",
    "params": {
    "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
    "prove": false
}
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
    id: 'getblock.io',
    method: 'tx',
    params: {
    "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
    "prove": false
}
}, { headers: { 'Content-Type': 'application/json' } });

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
        'id': 'getblock.io',
        'method': 'tx',
        'params': {
    "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
    "prove": false
}
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "tx",
            "params": {
    "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
    "prove": false
}
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00",
        "height": "12345678",
        "index": 0,
        "tx_result": {
            "code": 0,
            "gas_wanted": "200000",
            "gas_used": "118000",
            "events": [
                {
                    "type": "message",
                    "attributes": [
                        {
                            "key": "action",
                            "value": "/cosmos.bank.v1beta1.MsgSend"
                        }
                    ]
                }
            ]
        },
        "tx": "Cr0BC..."
    }
}
```

## Response Fields

| Field      | Type   | Description                            |
| ---------- | ------ | -------------------------------------- |
| height     | string | Block height the transaction landed in |
| tx\_result | object | ABCI result: code, gas, and events     |
| tx         | string | Base64-encoded raw transaction         |

## Use Cases

* **Status Checks**: Confirm a transaction succeeded via tx\_result.code
* **Receipt Reads**: Extract events emitted by the transaction
* **Explorer Backends**: Render transaction detail pages

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| -32603 / tx not found     | Not found     | No transaction matches the hash (or it is not yet committed) |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
