---
description: >-
  Example code for the get_header JSON-RPC method. Complete guide on how to use
  get_header JSON-RPC in GetBlock Web3 documentation.
---

# get\_header - Nervos

Returns the information about a block header by hash. Lighter than `get_block` when you only need header data — no transactions, proposals, or uncles.

## Parameters

| Parameter   | Type   | Required | Description                                                     |
| ----------- | ------ | -------- | --------------------------------------------------------------- |
| block\_hash | H256   | Yes      | Block hash                                                      |
| verbosity   | Uint32 | No       | `"0x1"` for full object (default), `"0x0"` for serialized bytes |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_header",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "0x1"
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
    "method": "get_header",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "0x1"
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
    "method": "get_header",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "0x1"
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
        "method": "get_header",
        "params": [
                "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
                "0x1"
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
        "compact_target": "0x1a08a97e",
        "dao": "0x40e8ea22a06a82822f9000c0d77b14002c5f3a0d4f8e4b07009fbd9f4d1f3909",
        "epoch": "0x707080a000ca6",
        "extra_hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "nonce": "0x37b54e3aef60b89321e7e8b3ce0f4c2d",
        "number": "0xe5a45f",
        "parent_hash": "0x77fdd22f6ae8a717de9ae2b128834e9b2a1bb7e7387f01ddc278c6ec37ebaa05",
        "proposals_hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "timestamp": "0x18f3b8d2a3c",
        "transactions_root": "0x6a7e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b",
        "version": "0x0"
    }
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                           |
| ------ | -------------- | ------------------------------------- |
| result | Header or null | Header object, or `null` if not found |

## Use Cases

* Lightweight chain-tip polling without fetching block bodies
* Header-only indexing in resource-constrained environments
* Light client header verification

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
const result = await rpc.send('get_header', ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "0x1"]);
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
    'method': 'get_header',
    'params': ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "0x1"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
