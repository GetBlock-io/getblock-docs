---
description: >-
  Example code for the get_transactions JSON-RPC method. Complete guide on how
  to use get_transactions JSON-RPC in GetBlock Web3 documentation.
---

# get\_transactions - Nervos

Returns transactions matching a lock or type script filter. Used to build transaction history for an address. Paginated; supports two response formats (`grouped` vs `ungrouped`).

## Parameters

| Parameter   | Type      | Required | Description                                                                        |
| ----------- | --------- | -------- | ---------------------------------------------------------------------------------- |
| search\_key | SearchKey | Yes      | Search criteria — `script`, `script_type`, optional filter, `group_by_transaction` |
| order       | string    | Yes      | `asc` or `desc`                                                                    |
| limit       | Uint32    | Yes      | Maximum results per page                                                           |
| after       | string    | No       | Cursor from previous response                                                      |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_transactions",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        },
        "desc",
        "0x32"
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
    "method": "get_transactions",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        },
        "desc",
        "0x32"
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
    "method": "get_transactions",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
            },
            "script_type": "lock"
        },
        "desc",
        "0x32"
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
        "method": "get_transactions",
        "params": [
                {
                        "script": {
                                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                                "hash_type": "type",
                                "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
                        },
                        "script_type": "lock"
                },
                "desc",
                "0x32"
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
        "objects": [
            {
                "block_number": "0xe5a45f",
                "io_index": "0x0",
                "io_type": "input",
                "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
                "tx_index": "0x1"
            }
        ],
        "last_cursor": "0x0080001000d39ae8b34e0d31e7d8b0b4b34e0d31e7d8b00000000000003"
    }
}
```
{% endcode %}

## Response Parameters

| Field                           | Type                               | Description                                                            |
| ------------------------------- | ---------------------------------- | ---------------------------------------------------------------------- |
| result.objects                  | array of TxRecord or TxGroupRecord | Matching transactions                                                  |
| result.objects\[].tx\_hash      | H256                               | Transaction hash                                                       |
| result.objects\[].block\_number | Uint64                             | Block number containing the transaction                                |
| result.objects\[].io\_type      | string                             | `input` or `output` — whether the script appeared in inputs or outputs |
| result.objects\[].io\_index     | Uint32                             | Index of the input/output in the transaction                           |
| result.last\_cursor             | string                             | Cursor for pagination                                                  |

## Use Cases

* Wallet transaction history for an address
* Building activity feeds for accounts
* Backfill of historical activity for indexers

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
const result = await rpc.send('get_transactions', [{"script": {"code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8", "hash_type": "type", "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"}, "script_type": "lock"}, "desc", "0x32"]);
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
    'method': 'get_transactions',
    'params': [{"script": {"code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8", "hash_type": "type", "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"}, "script_type": "lock"}, "desc", "0x32"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
