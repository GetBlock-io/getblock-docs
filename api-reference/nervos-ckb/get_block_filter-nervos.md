---
description: >-
  Example code for the get_block_filter JSON-RPC method. Complete guide on how
  to use get_block_filter JSON-RPC in GetBlock Web3 documentation.
---

# get\_block\_filter - Nervos

Returns the block filter data for a block. Block filters are compact summaries used by light clients to scan blocks without downloading their full contents.

## Parameters

| Parameter   | Type | Required | Description |
| ----------- | ---- | -------- | ----------- |
| block\_hash | H256 | Yes      | Block hash  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block_filter",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
    "method": "get_block_filter",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
    "method": "get_block_filter",
    "params": [
        "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
        "method": "get_block_filter",
        "params": [
                "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"
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
        "data": "0xabababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababab",
        "hash": "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
    }
}
```
{% endcode %}

## Response Parameters

| Field       | Type      | Description                                                         |
| ----------- | --------- | ------------------------------------------------------------------- |
| result.data | JsonBytes | Filter data — compact representation of cells touched in this block |
| result.hash | H256      | Hash of the filter data                                             |

## Use Cases

* Light clients scanning for relevant blocks without full download
* Mobile wallets that need efficient block scanning

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
const result = await rpc.send('get_block_filter', ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"]);
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
    'method': 'get_block_filter',
    'params': ["0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
