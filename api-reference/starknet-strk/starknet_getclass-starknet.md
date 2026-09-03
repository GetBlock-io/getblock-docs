---
description: >-
  Example code for the starknet_getClass JSON-RPC method. Complete guide on how
  to use starknet_getClass JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_getClass - STRK

This method returns the class definition — the Sierra program, entry points, and ABI — for a given class hash at a block. It is used to load a contract's interface and code.

## Parameters

| Parameter   | Type             | Required | Description                                |
| ----------- | ---------------- | -------- | ------------------------------------------ |
| block\_id   | object \| string | Yes      | {block\_number} or {block\_hash}, or a tag |
| class\_hash | string           | Yes      | The class hash to fetch                    |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getClass",
    "params": ["latest", "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003"],
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
    method: 'starknet_getClass',
    params: ['latest', '0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003'],
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
        'method': 'starknet_getClass',
        'params': ['latest', '0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003'],
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
            "method": "starknet_getClass",
            "params": ["latest", "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003"],
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

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Class definition (see fields below)     |

### Result Object

| Field                   | Type   | Description                                        |
| ----------------------- | ------ | -------------------------------------------------- |
| sierra\_program         | array  | The Sierra bytecode of the class                   |
| entry\_points\_by\_type | object | External, L1 handler, and constructor entry points |
| abi                     | string | JSON ABI string describing the class interface     |

## Use Cases

* **ABI Loading**: Load a contract's ABI to encode calls
* **Class Verification**: Compare a class definition against a known build
* **Tooling**: Feed Sierra/ABI into analysis or codegen tools
* **Interface Discovery**: Enumerate a contract's entry points

## Error Handling

| Error                        | Message              | Description                                      |
| ---------------------------- | -------------------- | ------------------------------------------------ |
| 28 / CLASS\_HASH\_NOT\_FOUND | Class hash not found | No class matches the supplied hash at that block |
| 24 / BLOCK\_NOT\_FOUND       | Block not found      | No block matches the supplied block\_id          |
