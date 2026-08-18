---
description: >-
  Example code for the getaddresstxids JSON-RPC method. Сomplete guide on how to
  use the getaddresstxids JSON-RPC method in GetBlock.io Web3 documentation.
---

# getaddresstxids - Zcash

This method returns the list of transaction IDs involving one or more transparent addresses within a specified block range. A Zcash extension over Bitcoin Core, requiring address indexing on the Zebra node. Transactions are returned in ascending block height order.

## Parameters

| Parameter | Type   | Required | Description                |
| --------- | ------ | -------- | -------------------------- |
| `request` | object | Yes      | Request object (see below) |

### Request Object

| Field       | Type            | Required | Description                                         |
| ----------- | --------------- | -------- | --------------------------------------------------- |
| `addresses` | array of string | Yes      | Transparent addresses to query                      |
| `start`     | integer         | No       | Starting block height (inclusive). Default `0`      |
| `end`       | integer         | No       | Ending block height (inclusive). Default: chain tip |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getaddresstxids",
    "params": [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
            ],
            "start": 2846342,
            "end": 2856342
        }
    ],
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
    method: 'getaddresstxids',
    params: [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
            ],
            "start": 2846342,
            "end": 2856342
        }
    ],
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
        'method': 'getaddresstxids',
        'params': [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
            ],
            "start": 2846342,
            "end": 2856342
        }
    ],
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
            "method": "getaddresstxids",
            "params": [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6"
            ],
            "start": 2846342,
            "end": 2856342
        }
    ],
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
    "result": [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "1cd7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b"
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                                      |
| --------- | --------------- | -------------------------------------------------------------------------------- |
| `result`  | array of string | Transaction IDs involving the queried addresses, in ascending block height order |

## Use Cases

* **Wallet Transaction History**: Reconstruct a wallet's complete on-chain transparent history
* **Explorer Address Activity**: Display all transactions touching an address on an explorer page
* **Compliance and Audit Trails**: Enumerate transactions for a family of addresses for AML compliance
* **Balance Reconstruction from Genesis**: Iterate all transactions to independently compute address balances

## Error Handling

| Error Code | Message         | Description                                      |
| ---------- | --------------- | ------------------------------------------------ |
| -8         | Invalid address | One or more addresses are malformed              |
| -32602     | Invalid params  | Block range is invalid                           |
| -32603     | Internal error  | Address indexing may be disabled on the endpoint |
