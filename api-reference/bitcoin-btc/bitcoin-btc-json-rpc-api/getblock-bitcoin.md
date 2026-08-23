# getblock bitcoin

This method returns information about a block given its hash. The amount of detail depends on the verbosity level: 0 returns hex, 1 a JSON object, and 2 a JSON object including full transaction data.

## Parameters

| Parameter | Type   | Required | Description                                                                            |
| --------- | ------ | -------- | -------------------------------------------------------------------------------------- |
| blockhash | string | Yes      | The block hash                                                                         |
| verbosity | number | No       | 0 for hex, 1 for a JSON object, 2 for a JSON object with transaction data (default: 1) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
    method: 'getblock',
    params: ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
        'method': 'getblock',
        'params': ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
            "method": "getblock",
            "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
        "hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "confirmations": 152,
        "height": 830000,
        "version": 671088644,
        "merkleroot": "e3a4b5c6d7e8f90112233445566778899aab1c2d3e4f5a6b7c8d9e0f1a2b3c4d",
        "time": 1706886000,
        "mediantime": 1706883000,
        "nonce": 3722946288,
        "bits": "17034219",
        "difficulty": 75502165623893.83,
        "chainwork": "0000000000000000000000000000000000000000682e0f1c1b2a3d4e5f6a7b8c",
        "nTx": 3201,
        "previousblockhash": "00000000000000000000f5f1c8b0d2e3a4b5c6d7e8f90112233445566778899a",
        "tx": [
            "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
        ],
        "strippedsize": 786543,
        "size": 1483920,
        "weight": 3943549
    }
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | object | Block object (fields depend on verbosity) |

### Result Object

| Field             | Type   | Description                                             |
| ----------------- | ------ | ------------------------------------------------------- |
| hash              | string | The block hash                                          |
| confirmations     | number | Number of confirmations, or -1 if not on the main chain |
| height            | number | The block height                                        |
| merkleroot        | string | The merkle root                                         |
| time              | number | Block time in seconds since the epoch                   |
| nTx               | number | Number of transactions in the block                     |
| tx                | array  | Transaction IDs (or objects when verbosity is 2)        |
| previousblockhash | string | Hash of the previous block                              |

## Use Cases

* **Block Explorers**: Render a block and its transactions
* **Indexing**: Ingest blocks and their transaction IDs
* **Confirmations**: Read how deep a block is buried
* **Auditing**: Inspect a specific block's contents

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
