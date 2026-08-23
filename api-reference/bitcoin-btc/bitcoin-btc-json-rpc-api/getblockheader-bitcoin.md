# getblockheader bitcoin

This method returns the block header for a given block hash. With `verbose=true` it returns a JSON object; with `verbose=false` it returns hex-encoded header data.

## Parameters

| Parameter | Type    | Required | Description                                           |
| --------- | ------- | -------- | ----------------------------------------------------- |
| blockhash | string  | Yes      | The block hash                                        |
| verbose   | boolean | No       | true for a JSON object, false for hex (default: true) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
    method: 'getblockheader',
    params: ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
        'method': 'getblockheader',
        'params': ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
            "method": "getblockheader",
            "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
        "previousblockhash": "00000000000000000000f5f1c8b0d2e3a4b5c6d7e8f90112233445566778899a",
        "nTx": 3201
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                  |
| id        | string | Request identifier matching the request            |
| result    | object | Block header object (or hex when verbose is false) |

### Result Object

| Field             | Type   | Description                           |
| ----------------- | ------ | ------------------------------------- |
| hash              | string | The block hash                        |
| height            | number | The block height                      |
| merkleroot        | string | The merkle root                       |
| time              | number | Block time in seconds since the epoch |
| bits              | string | The compact target in hex             |
| previousblockhash | string | Hash of the previous block            |

## Use Cases

* **Lightweight Sync**: Read headers without full blocks
* **Difficulty Tracking**: Read bits and difficulty per block
* **Chain Following**: Follow the chain by header
* **Timestamp Checks**: Read block time and median time

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
