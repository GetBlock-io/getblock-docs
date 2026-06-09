---
description: >-
  Example code for the get_epoch_by_number JSON-RPC method. Complete guide on
  how to use get_epoch_by_number JSON-RPC in GetBlock Web3 documentation.
---

# get\_epoch\_by\_number - Nervos

Returns the epoch in the canonical chain with the specified number.

## Parameters

| Parameter     | Type   | Required | Description                               |
| ------------- | ------ | -------- | ----------------------------------------- |
| epoch\_number | Uint64 | Yes      | Epoch number (hex string, e.g. `"0xca6"`) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_epoch_by_number",
    "params": [
        "0xca6"
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
    "method": "get_epoch_by_number",
    "params": [
        "0xca6"
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
    "method": "get_epoch_by_number",
    "params": [
        "0xca6"
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
        "method": "get_epoch_by_number",
        "params": [
                "0xca6"
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
        "length": "0x708",
        "number": "0xca6",
        "start_number": "0xe5a000"
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type   | Description                               |
| ---------------------- | ------ | ----------------------------------------- |
| result.number          | Uint64 | Epoch number (echoed)                     |
| result.length          | Uint64 | Number of blocks in this epoch            |
| result.start\_number   | Uint64 | First block number in this epoch          |
| result.compact\_target | Uint32 | Difficulty target effective in this epoch |

## Use Cases

* Historical epoch analysis
* Computing block ranges that fall within an epoch
* Auditing difficulty adjustments over time

## Error Handling

| Status Code | Error Message        | Cause                                                |
| ----------- | -------------------- | ---------------------------------------------------- |
| 403         | Forbidden            | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params       | Request parameters are missing or malformed          |
| -32601      | Method not found     | Method does not exist or is not enabled on this node |
| -32603      | Internal error       | Server-side error while processing the request       |
| 429         | Too Many Requests    | Rate limit exceeded for your plan                    |
| -32602      | Invalid epoch number | Epoch number is above the current epoch              |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_epoch_by_number', ["0xca6"]);
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
    'method': 'get_epoch_by_number',
    'params': ["0xca6"]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
