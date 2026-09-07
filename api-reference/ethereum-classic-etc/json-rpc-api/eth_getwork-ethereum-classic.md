# eth\_getWork - Ethereum Classic

This method returns the current Proof-of-Work work package: the current block header pow-hash, the seed hash used for the DAG, and the boundary condition (target). Miners use it to find a valid nonce.

## Parameters

{% hint style="info" %}
This method does not require any parameters. Send the request with an empty `params` array.
{% endhint %}

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getWork",
    "params": [],
    "id": "getblock.io"
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
    method: 'eth_getWork',
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
        'method': 'eth_getWork',
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
            "method": "eth_getWork",
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
    "result": [
        "0x1bdf...c3a2",
        "0x5a3f...9e11",
        "0x0000000583abc..."
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| jsonrpc   | string | JSON-RPC version ("2.0")                        |
| id        | string | Request identifier                              |
| result    | array  | \[current pow-hash, seed hash, boundary target] |

## Use Cases

* **Mining**: Fetch work for an external miner
* **Pool Software**: Distribute work to workers
* **Diagnostics**: Inspect the current target

## Error Handling

| Error Code                            | Message       | Description                                       |
| ------------------------------------- | ------------- | ------------------------------------------------- |
| -32000 / no mining work available yet | No work       | The node is not ready to serve work               |
| 403 / RBAC: access denied             | Access denied | The GetBlock access token is missing or incorrect |
