---
description: >-
  Example code for the get_cells_capacity JSON-RPC method. Complete guide on how
  to use get_cells_capacity JSON-RPC in GetBlock Web3 documentation.
---

# get\_cells\_capacity - Nervos

Returns the total capacity of live cells matching a lock or type script filter. **This is the canonical balance query for CKB** — pass the lock script for the address and the response is the total CKB balance in Shannon.

## Parameters

| Parameter   | Type      | Required | Description                                                |
| ----------- | --------- | -------- | ---------------------------------------------------------- |
| search\_key | SearchKey | Yes      | Search criteria — `script`, `script_type`, optional filter |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_cells_capacity",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        }
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_cells_capacity",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        }
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_cells_capacity",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        }
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "get_cells_capacity",
        "params": [
                {
                        "script": {
                                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                                "hash_type": "type",
                                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
                        },
                        "script_type": "lock"
                }
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "block_number": "0xe5a45f",
        "capacity": "0xe8d4a51000"
    }
}
```
{% endcode %}

## Response Parameters

| Field                | Type   | Description                                      |
| -------------------- | ------ | ------------------------------------------------ |
| result.capacity      | Uint64 | Total capacity of matching live cells in Shannon |
| result.block\_hash   | H256   | Block hash at which the capacity was computed    |
| result.block\_number | Uint64 | Block number at which the capacity was computed  |

## Use Cases

* Wallet balance display (the most common CKB query)
* Pre-flight funds checks before constructing a transaction
* Portfolio applications tracking address balances

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_cells_capacity', [{"script": {"code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8", "hash_type": "type", "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"}, "script_type": "lock"}]);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'get_cells_capacity',
    'params': [{"script": {"code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8", "hash_type": "type", "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"}, "script_type": "lock"}]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
