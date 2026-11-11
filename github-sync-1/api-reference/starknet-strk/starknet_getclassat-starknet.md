---
description: >-
  Example code for the starknet_getClassAt JSON-RPC method. Complete guide on
  how to use starknet_getClassAt JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_getClassAt - STRK

This method returns the class definition of the contract deployed at a given address at a block, resolving the class hash for you. It combines class-hash lookup and class fetch.

## Parameters

| Parameter         | Type             | Required | Description                                |
| ----------------- | ---------------- | -------- | ------------------------------------------ |
| block\_id         | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag |
| contract\_address | string           | Yes      | The contract address to resolve and fetch  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getClassAt",
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
    method: 'starknet_getClassAt',
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
        'method': 'starknet_getClassAt',
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
            "method": "starknet_getClassAt",
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
    "result": {
        "contract_class_version": "0.1.0",
        "sierra_program": [
            "0x1",
            "0x3",
            "0x..."
        ],
        "entry_points_by_type": {
            "EXTERNAL": [
                {
                    "selector": "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e",
                    "function_idx": 0
                }
            ],
            "L1_HANDLER": [],
            "CONSTRUCTOR": []
        },
        "abi": "[{\"type\":\"function\",\"name\":\"balanceOf\"}]"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")         |
| id        | string | Request identifier matching the request   |
| result    | object | Class definition of the deployed contract |

## Use Cases

* **One-Call ABI**: Fetch a deployed contract's ABI by address
* **Interface Discovery**: Enumerate entry points for an address
* **Tooling**: Load code and ABI directly from an address
* **Verification**: Confirm a deployed contract's class contents

## Error Handling

| Error                     | Message            | Description                                     |
| ------------------------- | ------------------ | ----------------------------------------------- |
| 20 / CONTRACT\_NOT\_FOUND | Contract not found | No contract exists at the address at that block |
| 24 / BLOCK\_NOT\_FOUND    | Block not found    | No block matches the supplied block\_id         |
