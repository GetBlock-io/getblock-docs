# starknet\_getStateUpdate starknet

This method returns the state update for a block: the storage diffs, deployed contracts, declared classes, and nonces that changed. It is used to build state-diff indexers.

## Parameters

| Parameter | Type             | Required | Description                                                      |
| --------- | ---------------- | -------- | ---------------------------------------------------------------- |
| block\_id | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag ("latest", "pending") |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getStateUpdate",
    "params": [{ "block_number": 700000 }],
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
    method: 'starknet_getStateUpdate',
    params: [{ block_number: 700000 }],
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
        'method': 'starknet_getStateUpdate',
        'params': [{ 'block_number': 700000 }],
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
            "method": "starknet_getStateUpdate",
            "params": [{ "block_number": 700000 }],
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
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "old_root": "0x0525f...",
        "new_root": "0x0713a...",
        "state_diff": {
            "storage_diffs": [
                {
                    "address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7",
                    "storage_entries": [
                        {
                            "key": "0x0206f38f7e4f15e87567361213c28f235cccdaa1d7fd34c9db1dfe9489c6a091",
                            "value": "0x2a"
                        }
                    ]
                }
            ],
            "deployed_contracts": [],
            "declared_classes": [],
            "nonces": [
                {
                    "contract_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
                    "nonce": "0x2b"
                }
            ]
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")           |
| id        | string | Request identifier matching the request     |
| result    | object | Roots and the state diff (see fields below) |

### Result Object

| Field       | Type   | Description                                                            |
| ----------- | ------ | ---------------------------------------------------------------------- |
| new\_root   | string | State root after applying the block                                    |
| state\_diff | object | Storage diffs, deployed contracts, declared classes, and nonce changes |

## Use Cases

* **State Indexing**: Drive an indexer from per-block state diffs
* **Storage Watching**: Detect storage changes for specific contracts
* **Nonce Tracking**: Follow account nonce changes
* **Proof Systems**: Read old\_root/new\_root for verification

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 24 / BLOCK\_NOT\_FOUND    | Block not found | No block matches the supplied block\_id           |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
