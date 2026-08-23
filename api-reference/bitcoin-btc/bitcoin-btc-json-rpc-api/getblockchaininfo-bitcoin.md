# getblockchaininfo bitcoin

This method returns an object with state information about blockchain processing, including the chain, height, best block hash, difficulty, verification progress, and softfork status.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockchaininfo",
    "params": [],
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
    method: 'getblockchaininfo',
    params: [],
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
        'method': 'getblockchaininfo',
        'params': [],
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
            "method": "getblockchaininfo",
            "params": [],
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
        "chain": "main",
        "blocks": 830000,
        "headers": 830000,
        "bestblockhash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "difficulty": 75502165623893.83,
        "time": 1706886000,
        "mediantime": 1706883000,
        "verificationprogress": 0.9999978,
        "initialblockdownload": false,
        "chainwork": "0000000000000000000000000000000000000000682e0f1c1b2a3d4e5f6a7b8c",
        "size_on_disk": 623456789012,
        "pruned": false
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Blockchain state object                 |

### Result Object

| Field                | Type    | Description                                        |
| -------------------- | ------- | -------------------------------------------------- |
| chain                | string  | Current network name (main, test, signet, regtest) |
| blocks               | number  | Height of the most-work fully-validated chain      |
| headers              | number  | Current number of headers validated                |
| bestblockhash        | string  | Hash of the current best block                     |
| difficulty           | number  | Current mining difficulty                          |
| verificationprogress | number  | Estimate of verification progress (0 to 1)         |
| pruned               | boolean | Whether the node is in pruned mode                 |

## Use Cases

* **Node Status**: Read the chain, height, and sync progress
* **Readiness Checks**: Confirm the node is fully synced
* **Dashboards**: Display chain state and difficulty
* **Network Detection**: Branch logic on the chain name

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
