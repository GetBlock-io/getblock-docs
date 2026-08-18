---
description: >-
  Example code for the getaddressbalance JSON-RPC method. Сomplete guide on how
  to use the getaddressbalance JSON-RPC method in GetBlock.io Web3
  documentation.
---

# getaddressbalance - Zcash

This method returns the total transparent balance of one or more addresses. A Zcash extension over Bitcoin Core (which doesn't natively index addresses), available on Zebra when address indexing is enabled. **Transparent addresses only** — shielded balances (Sapling, Orchard) are private by design and cannot be queried via public RPC.

## Parameters

| Parameter             | Type            | Required | Description                                                                |
| --------------------- | --------------- | -------- | -------------------------------------------------------------------------- |
| `addresses_or_object` | object or array | Yes      | Either an object `{"addresses": [...]}` or a bare array of address strings |

### Address Object

| Field       | Type            | Required | Description                                       |
| ----------- | --------------- | -------- | ------------------------------------------------- |
| `addresses` | array of string | Yes      | Transparent addresses (t1/t3) to sum balances for |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getaddressbalance",
    "params": [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                "t1KG5xEeywXupj9dJHTvHzT5eN4RvpxaAJc"
            ]
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
    method: 'getaddressbalance',
    params: [
        {
            "addresses": [
                "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                "t1KG5xEeywXupj9dJHTvHzT5eN4RvpxaAJc"
            ]
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
        'method': 'getaddressbalance',
        'params': [
            {
                "addresses": [
                    "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                    "t1KG5xEeywXupj9dJHTvHzT5eN4RvpxaAJc"
                ]
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
            "method": "getaddressbalance",
            "params": [
                {
                    "addresses": [
                        "t1c74RiTicVSKthLmn2c4WFLJdLDs5sQ4X6",
                        "t1KG5xEeywXupj9dJHTvHzT5eN4RvpxaAJc"
                    ]
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
    "result": {
        "balance": 274999000,
        "received": 500000000
    }
}
```

## Response Parameters

| Parameter  | Type    | Description                                                             |
| ---------- | ------- | ----------------------------------------------------------------------- |
| `balance`  | integer | Current unspent balance in zatoshi (1 ZEC = 10^8 zat)                   |
| `received` | integer | Total amount ever received in zatoshi (including already-spent outputs) |

## Use Cases

* **Wallet Balance Display**: Show a transparent balance for one or many addresses in a wallet UI
* **Aggregated Balance Tracking**: Sum balances across a family of addresses in one call
* **Explorer Address Pages**: Display current balance and lifetime received amounts on address detail pages
* **Custody System Reconciliation**: Query balances for all custody addresses in bulk

## Error Handling

| Error Code | Message         | Description                                                                         |
| ---------- | --------------- | ----------------------------------------------------------------------------------- |
| -8         | Invalid address | One or more addresses are malformed or not transparent                              |
| -32602     | Invalid params  | Request structure is malformed                                                      |
| -32603     | Internal error  | Node failed to look up address balances (may indicate address indexing is disabled) |
