---
description: >-
  Example code for the get_live_cell JSON-RPC method. Complete guide on how to
  use get_live_cell JSON-RPC in GetBlock Web3 documentation.
---

# get\_live\_cell - Nervos

Returns the live cell at the given out point. "Live" means the cell exists in the chain and has not been spent. Optionally include the cell's data for inspection.

## Parameters

| Parameter  | Type     | Required | Description                                            |
| ---------- | -------- | -------- | ------------------------------------------------------ |
| out\_point | OutPoint | Yes      | Out point — `tx_hash` and `index` identifying the cell |
| with\_data | boolean  | Yes      | If `true`, include the cell's data field               |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_live_cell",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        true
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
    "method": "get_live_cell",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        true
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
    "method": "get_live_cell",
    "params": [
        {
            "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
            "index": "0x0"
        },
        true
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
        "method": "get_live_cell",
        "params": [
                {
                        "tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0",
                        "index": "0x0"
                },
                true
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
        "cell": {
            "data": {
                "content": "0x",
                "hash": "0x44f5e6f8b0c8f3a2e1d4b7c0e3f6a9d2b5c8e1f4a7d0b3c6e9f2a5d8b1c4e7f0"
            },
            "output": {
                "capacity": "0xe8d4a51000",
                "lock": {
                    "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                    "hash_type": "type",
                    "args": "0xb39d53656421a3895eb35a627d4a6e76dd4afba6"
                },
                "type": null
            }
        },
        "status": "live"
    }
}
```
{% endcode %}

## Response Parameters

| Field                       | Type           | Description                                         |
| --------------------------- | -------------- | --------------------------------------------------- |
| result.cell.output.capacity | Uint64         | Cell capacity in Shannon                            |
| result.cell.output.lock     | Script         | Lock script (controls who can spend the cell)       |
| result.cell.output.type     | Script \| null | Type script (governs cell type semantics) or `null` |
| result.cell.data            | CellData       | Cell data if `with_data=true`                       |
| result.status               | string         | `live`, `dead`, or `unknown`                        |

## Use Cases

* Reading cell contents during transaction construction
* Verifying a cell is still spendable
* DEX, lending, and other on-chain apps inspecting cell state

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
const result = await rpc.send('get_live_cell', [{"tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "index": "0x0"}, true]);
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
    'method': 'get_live_cell',
    'params': [{"tx_hash": "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0", "index": "0x0"}, true]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
