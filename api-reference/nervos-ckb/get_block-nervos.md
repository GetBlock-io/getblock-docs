---
description: >-
  Example code for the get_block JSON-RPC method. Complete guide on how to use
  get_block JSON-RPC in GetBlock Web3 documentation.
---

# get\_block - Nervos

Returns the information about a block by hash. The `verbosity` parameter controls return format: `"0x2"` (default) returns a full block object; `"0x0"` returns the serialized molecule-encoded bytes. The `with_cycles` parameter optionally returns per-transaction cycle counts.

## Parameters

| Parameter    | Type    | Required | Description                                                     |
| ------------ | ------- | -------- | --------------------------------------------------------------- |
| block\_hash  | H256    | Yes      | Block hash                                                      |
| verbosity    | Uint32  | No       | `"0x2"` for full object (default), `"0x0"` for serialized bytes |
| with\_cycles | boolean | No       | If `true`, include per-transaction cycles (default false)       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
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
    "method": "get_block",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
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
    "method": "get_block",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
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
        "method": "get_block",
        "params": [
                "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "header": {
            "hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
            "number": "0xe5a45f",
            "timestamp": "0x18f3b8d2a3c",
            "parent_hash": "0x77fdd22f6ae8a717de9ae2b128834e9b2a1bb7e7387f01ddc278c6ec37ebaa05",
            "epoch": "0x707080a000ca6"
        },
        "proposals": [],
        "transactions": [
            {
                "hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
                "version": "0x0",
                "cell_deps": [],
                "header_deps": [],
                "inputs": [],
                "outputs": [],
                "outputs_data": [],
                "witnesses": []
            }
        ],
        "uncles": []
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type                     | Description                                                                    |
| ------------------- | ------------------------ | ------------------------------------------------------------------------------ |
| result.header       | Header                   | Block header (same shape as `get_tip_header`)                                  |
| result.transactions | array of Transaction     | Transactions included in the block (first is always the cellbase)              |
| result.proposals    | array of ProposalShortId | Transactions proposed in this block (10-byte IDs for the 2-step commit window) |
| result.uncles       | array of UncleBlock      | Uncle blocks referenced by this block                                          |

## Use Cases

* Full block indexing in explorers
* Replaying blocks for analytics
* Inspecting all transactions in a block

## Error Handling

| Status Code   | Error Message     | Cause                                                               |
| ------------- | ----------------- | ------------------------------------------------------------------- |
| 403           | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                 |
| -32602        | Invalid params    | Request parameters are missing or malformed                         |
| -32601        | Method not found  | Method does not exist or is not enabled on this node                |
| -32603        | Internal error    | Server-side error while processing the request                      |
| 429           | Too Many Requests | Rate limit exceeded for your plan                                   |
| (null result) | Block not found   | No block with this hash exists, or it is not in the canonical chain |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_block', ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "0x2", false]);
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
    'method': 'get_block',
    'params': ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "0x2", false]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
