# eth\_submitWork - Ethereum Classic

This method submits a Proof-of-Work solution — the found nonce, the pow-hash of the header, and the mix digest — and returns whether the solution was accepted as valid.

## Parameters

| Parameter | Type   | Required | Description                         |
| --------- | ------ | -------- | ----------------------------------- |
| nonce     | string | Yes      | 8-byte found nonce (hex)            |
| powHash   | string | Yes      | Header pow-hash the solution is for |
| mixDigest | string | Yes      | 32-byte mix digest                  |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_submitWork",
    "params": ["0x0000000000000001", "0x1bdf...c3a2", "0x5a3f...9e11"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_submitWork',
    params: ['0x0000000000000001', '0x1bdf...c3a2', '0x5a3f...9e11'],
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
        'method': 'eth_submitWork',
        'params': ['0x0000000000000001', '0x1bdf...c3a2', '0x5a3f...9e11'],
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
            "method": "eth_submitWork",
            "params": ["0x0000000000000001", "0x1bdf...c3a2", "0x5a3f...9e11"],
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                 |
| --------- | ------- | ------------------------------------------- |
| jsonrpc   | string  | JSON-RPC version ("2.0")                    |
| id        | string  | Request identifier                          |
| result    | boolean | true if the solution was valid and accepted |

## Use Cases

* **Mining**: Submit a found block solution
* **Pool Software**: Relay worker solutions
* **Validation**: Confirm a solution was accepted

## Error Handling

| Error Code                 | Message           | Description                                       |
| -------------------------- | ----------------- | ------------------------------------------------- |
| -32602 / invalid arguments | Invalid arguments | The nonce, pow-hash, or mix digest is malformed   |
| 403 / RBAC: access denied  | Access denied     | The GetBlock access token is missing or incorrect |
