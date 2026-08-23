# joinpsbts bitcoin

This method joins multiple distinct PSBTs into a single PSBT by concatenating their inputs and outputs. The PSBTs must not share inputs.

## Parameters

| Parameter | Type  | Required | Description                      |
| --------- | ----- | -------- | -------------------------------- |
| txs       | array | Yes      | The base64-encoded PSBTs to join |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "joinpsbts",
    "params": [["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA==", "cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="]],
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
    method: 'joinpsbts',
    params: [["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA==", "cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="]],
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
        'method': 'joinpsbts',
        'params': [["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA==", "cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="]],
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
            "method": "joinpsbts",
            "params": [["cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA==", "cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="]],
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
    "result": "cHNidP8BAHUCAAAAASaBcTce3/KF6Tet7qSze3gADAVmy7OtZGQXE8pCFxv2AAAAAAD+////AtPf9QUAAAAAGXapFPUKlXhpm/rZ4JBKZ2n3nQ4jCMYBiKz1AAAAAA=="
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | The joined base64-encoded PSBT          |

## Use Cases

* **Coinjoin**: Combine inputs and outputs from multiple parties
* **Payment Batching**: Merge separate PSBTs into one transaction
* **Collaboration**: Assemble a shared transaction
* **Privacy Tools**: Build joined transactions

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                |
| -22        | Decode failed     | The transaction or script could not be decoded |
| -8         | Invalid parameter | A parameter is out of range or malformed       |
| -32602     | Invalid params    | A parameter is missing or has the wrong type   |
