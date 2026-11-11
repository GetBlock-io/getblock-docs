---
description: >-
  Example code for the getaddresstxids JSON-RPC method. Complete guide on how to
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
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "start": 3487700,
            "end": 3487790
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
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "start": 3487700,
            "end": 3487790
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
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "start": 3487700,
            "end": 3487790
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
                "t1KfwsnwJeNRVjQGBDZhwKskpQbih2qx5Ua"
            ],
            "start": 3487700,
            "end": 3487790
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
    "result": [
        "55b5dcc579e59f57a703b701d66c0176a22975544d3b24aac68c682650383e38",
        "d23fe67e3bab491b26999eac332412fa9176653adad8c0677d74699965e0055f"
    ],
    "id": "getblock.io"
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
