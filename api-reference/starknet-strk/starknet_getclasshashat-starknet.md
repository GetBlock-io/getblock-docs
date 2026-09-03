# starknet\_getClassHashAt starknet

This method returns the class hash of the contract deployed at a given address at a specific block. It identifies which class an address runs.

## Parameters

| Parameter         | Type             | Required | Description                                |
| ----------------- | ---------------- | -------- | ------------------------------------------ |
| block\_id         | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag |
| contract\_address | string           | Yes      | The contract address to query              |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getClassHashAt",
    "params": ["latest", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7"],
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
    method: 'starknet_getClassHashAt',
    params: ['latest', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7'],
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
        'method': 'starknet_getClassHashAt',
        'params': ['latest', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7'],
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
            "method": "starknet_getClassHashAt",
            "params": ["latest", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7"],
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
    "result": "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | string | Class hash deployed at the address      |

## Use Cases

* **Implementation Detection**: Identify which class an address runs
* **Upgrade Tracking**: Detect class-hash changes after an upgrade
* **Wallet Detection**: Recognize known account class hashes
* **Verification**: Match a deployed address to an expected class

## Error Handling

| Error                     | Message            | Description                                     |
| ------------------------- | ------------------ | ----------------------------------------------- |
| 20 / CONTRACT\_NOT\_FOUND | Contract not found | No contract exists at the address at that block |
| 24 / BLOCK\_NOT\_FOUND    | Block not found    | No block matches the supplied block\_id         |
