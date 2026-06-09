---
description: >-
  Example code for the get_transaction JSON-RPC method. Complete guide on how to
  use get_transaction JSON-RPC in GetBlock Web3 documentation.
---

# get\_transaction - Nervos

Returns the information about a transaction by hash, including its TxStatus (`Pending`, `Proposed`, `Committed`, `Unknown`, `Rejected`) and, if committed, the block hash containing it.

## Parameters

| Parameter       | Type    | Required | Description                                                                                          |
| --------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------- |
| tx\_hash        | H256    | Yes      | Transaction hash                                                                                     |
| verbosity       | Uint32  | No       | `"0x2"` for full transaction object (default), `"0x0"` for serialized bytes, `"0x1"` for status only |
| only\_committed | boolean | No       | If `true`, only return committed transactions                                                        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_transaction",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
        "0x2",
        false
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
    "method": "get_transaction",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
        "0x2",
        false
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
    "method": "get_transaction",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
        "0x2",
        false
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
        "method": "get_transaction",
        "params": [
                "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
                "0x2",
                false
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "transaction": {
            "version": "0x0",
            "hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "cell_deps": [
                {
                    "out_point": {
                        "tx_hash": "0x71a7ba8fc96349fea0ed3a5c47992e3b4084b031a42264a018e0072e8172e46c",
                        "index": "0x0"
                    },
                    "dep_type": "dep_group"
                }
            ],
            "header_deps": [],
            "inputs": [
                {
                    "previous_output": {
                        "tx_hash": "0xc8b07a3d2c4f5e6d7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f",
                        "index": "0x0"
                    },
                    "since": "0x0"
                }
            ],
            "outputs": [
                {
                    "capacity": "0xe8d4a51000",
                    "lock": {
                        "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                        "hash_type": "type",
                        "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
                    },
                    "type": null
                }
            ],
            "outputs_data": [
                "0x"
            ],
            "witnesses": [
                "0x55000000100000005500000055000000410000003e7c4b66c1c4e4b27b48e1c0c2e4f7a1b9c8d3e6f5a2b1d4e7c0f8a3b5d2e6c1f4a8c0e2d5b7e9f3a1c4b6d8e0f2a5c7d9e1b3f5a7c9e0b2d4f6a8c0e2b4d6f01"
            ]
        },
        "tx_status": {
            "status": "committed",
            "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
            "block_number": "0xe5a45f",
            "reason": null
        },
        "cycles": "0x3a980",
        "fee": "0x3e8",
        "min_replace_fee": null,
        "time_added_to_pool": null
    }
}
```

## Response Parameters

| Field                           | Type                | Description                                                      |
| ------------------------------- | ------------------- | ---------------------------------------------------------------- |
| result.transaction              | Transaction \| null | Transaction body, or `null` if rejected or unknown               |
| result.tx\_status.status        | string              | One of `pending`, `proposed`, `committed`, `unknown`, `rejected` |
| result.tx\_status.block\_hash   | H256 \| null        | Block containing the transaction if committed                    |
| result.tx\_status.block\_number | Uint64 \| null      | Block number if committed                                        |
| result.cycles                   | Uint64 \| null      | CKB-VM cycles consumed during verification (if available)        |
| result.fee                      | Uint64 \| null      | Transaction fee in Shannon                                       |
| result.min\_replace\_fee        | Uint64 \| null      | Minimum fee needed to replace this transaction in the pool (RBF) |

## Use Cases

* Confirming transaction inclusion after broadcast
* Wallet transaction history
* Detecting rejected transactions and their reasons
* Replace-by-fee (RBF) bumping logic

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
const result = await rpc.send('get_transaction', ["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "0x2", false]);
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
    'method': 'get_transaction',
    'params': ["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "0x2", false]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
