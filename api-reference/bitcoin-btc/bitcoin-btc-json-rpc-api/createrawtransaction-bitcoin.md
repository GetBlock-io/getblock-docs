---
description: >-
  Example code for the createrawtransaction JSON-RPC method. Complete guide on
  how to use createrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# createrawtransaction - Bitcoin

This method creates an unsigned raw transaction from a set of inputs and outputs and returns it as hex. It does not sign the transaction or broadcast it.

## Parameters

| Parameter   | Type    | Required | Description                                           |
| ----------- | ------- | -------- | ----------------------------------------------------- |
| inputs      | array   | Yes      | Inputs, each with a txid and vout                     |
| outputs     | array   | Yes      | Outputs mapping addresses to amounts, or data outputs |
| locktime    | number  | No       | Raw locktime (default: 0)                             |
| replaceable | boolean | No       | Whether the transaction signals RBF (default: false)  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "createrawtransaction",
    "params": [[{"txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", "vout": 0}], [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 6.24}]],
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
    method: 'createrawtransaction',
    params: [[{"txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", "vout": 0}], [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 6.24}]],
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
        'method': 'createrawtransaction',
        'params': [[{"txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", "vout": 0}], [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 6.24}]],
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
            "method": "createrawtransaction",
            "params": [[{"txid": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b", "vout": 0}], [{"bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq": 6.24}]],
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
    "result": "0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")          |
| id        | string | Request identifier matching the request    |
| result    | string | The unsigned transaction serialized as hex |

## Use Cases

* **Transaction Building**: Assemble inputs and outputs off-node
* **Custody Systems**: Build transactions for offline signing
* **Batching**: Construct multi-output payments
* **PSBT Flows**: Create a base transaction to convert to PSBT

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
