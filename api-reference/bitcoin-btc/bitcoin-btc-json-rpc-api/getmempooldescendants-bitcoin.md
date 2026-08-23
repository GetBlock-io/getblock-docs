# getmempooldescendants bitcoin

This method returns the in-mempool descendants of a transaction: the unconfirmed transactions that depend on it.

## Parameters

| Parameter | Type    | Required | Description                                                           |
| --------- | ------- | -------- | --------------------------------------------------------------------- |
| txid      | string  | Yes      | The transaction ID                                                    |
| verbose   | boolean | No       | true for detailed objects, false for a list of txids (default: false) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempooldescendants",
    "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", false],
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
    method: 'getmempooldescendants',
    params: ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", false],
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
        'method': 'getmempooldescendants',
        'params': ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", false],
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
            "method": "getmempooldescendants",
            "params": ["4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", false],
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
    "result": [
        "9f8e7d6c5b4a39281706f5e4d3c2b1a09f8e7d6c5b4a39281706f5e4d3c2b1a0"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                   |
| id        | string | Request identifier matching the request             |
| result    | array  | Array of descendant txids (or objects when verbose) |

## Use Cases

* **CPFP Planning**: Read the descendants depending on a parent
* **Eviction Analysis**: Understand what a replacement would evict
* **Dependency Analysis**: Map unconfirmed descendants
* **Wallets**: Warn about long descendant chains

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
