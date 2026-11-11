---
description: >-
  Example code for the block_results JSON-RPC method. Complete guide on how to
  use block_results JSON-RPC in GetBlock Web3 documentation.
---

# block\_results - Cronos

Returns the execution results for every transaction in a block — ABCI codes, gas, and emitted events — along with block-level events and any validator or consensus parameter updates. This is the primary surface for event-driven indexers reading what actually happened in a block.

## Parameters

| Parameter | Type   | Required | Description                                         |
| --------- | ------ | -------- | --------------------------------------------------- |
| height    | string | Optional | Block height as a string; omit for the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "block_results",
    "params": {
        "height": "12345678"
    }
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
    id: 'getblock.io',
    method: 'block_results',
    params: {
        "height": "12345678"
    }
}, { headers: { 'Content-Type': 'application/json' } });

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
        'id': 'getblock.io',
        'method': 'block_results',
        'params': {
            "height": "12345678"
        }
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "block_results",
            "params": {
                "height": "12345678"
            }
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "height": "12345678",
        "txs_results": [
            {
                "code": 0,
                "data": "EisKKS9jb3Ntb3MuYmFuay52MWJldGExLk1zZ1NlbmRSZXNwb25zZQ==",
                "log": "",
                "gas_wanted": "200000",
                "gas_used": "118000",
                "events": [
                    {
                        "type": "transfer",
                        "attributes": [
                            {
                                "key": "recipient",
                                "value": "crc1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
                            },
                            {
                                "key": "amount",
                                "value": "100000basecro"
                            }
                        ]
                    }
                ]
            }
        ],
        "finalize_block_events": [],
        "validator_updates": [],
        "consensus_param_updates": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            }
        }
    }
}
```

## Response Fields

| Field                        | Type    | Description                                                   |
| ---------------------------- | ------- | ------------------------------------------------------------- |
| height                       | string  | Block height the results belong to                            |
| txs\_results                 | array   | Per-transaction ABCI execution results, in block order        |
| txs\_results\[].code         | integer | ABCI code: 0 on success, non-zero on failure                  |
| txs\_results\[].gas\_used    | string  | Gas actually consumed by the transaction                      |
| txs\_results\[].events       | array   | Events emitted by the transaction — the event-sourcing surface |
| finalize\_block\_events      | array   | Events emitted by the block's begin/end app phases            |
| validator\_updates           | array   | Validator set changes applied at this block                   |

## Use Cases

* **Event Indexing**: Extract transfer, staking, and module events per block
* **Failure Analysis**: Find failed transactions via txs\_results\[].code
* **Gas Accounting**: Sum gas\_used across a block for fee analytics

## Error Handling

| Error                     | Message        | Description                                                 |
| ------------------------- | -------------- | ----------------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height is above the current tip or below the retained range |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
