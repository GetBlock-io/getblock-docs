# abci\_query - Cronos

Executes an ABCI query against application state at a given path, returning the raw (base64) value and, optionally, a proof. This is the low-level way to read any Cosmos SDK module store, and underlies the REST and gRPC gateways.

## Parameters

| Parameter | Type    | Required | Description                                    |
| --------- | ------- | -------- | ---------------------------------------------- |
| path      | string  | Yes      | ABCI query path, e.g. a gRPC query method path |
| data      | string  | Yes      | Hex-encoded query request bytes                |
| height    | string  | Optional | Height to query (0 or omitted = latest)        |
| prove     | boolean | Optional | Include an IAVL proof                          |

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
    "method": "abci_query",
    "params": {
    "path": "/cosmos.bank.v1beta1.Query/AllBalances",
    "data": "0a2b...",
    "height": "0",
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
    method: 'abci_query',
    params: {
    "path": "/cosmos.bank.v1beta1.Query/AllBalances",
    "data": "0a2b...",
    "height": "0",
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
        'method': 'abci_query',
        'params': {
    "path": "/cosmos.bank.v1beta1.Query/AllBalances",
    "data": "0a2b...",
    "height": "0",
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
            "method": "abci_query",
            "params": {
    "path": "/cosmos.bank.v1beta1.Query/AllBalances",
    "data": "0a2b...",
    "height": "0",
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
        "response": {
            "code": 0,
            "value": "Cg8KB2Jhc2Vjcm8SBDEwMDA=",
            "height": "12345678",
            "log": ""
        }
    }
}
```

## Response Fields

| Field           | Type    | Description                            |
| --------------- | ------- | -------------------------------------- |
| response.value  | string  | Base64-encoded protobuf query response |
| response.code   | integer | ABCI code; 0 on success                |
| response.height | string  | Height the query was answered at       |

## Use Cases

* **Generic Module Reads**: Query any module store not exposed elsewhere
* **Proofs**: Fetch state with an IAVL proof via prove=true
* **Light Clients**: Read verified state at a specific height

## Error Handling

| Error                     | Message        | Description                                         |
| ------------------------- | -------------- | --------------------------------------------------- |
| -32603 / Internal error   | Internal error | The path is unknown or the data bytes are malformed |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect   |
