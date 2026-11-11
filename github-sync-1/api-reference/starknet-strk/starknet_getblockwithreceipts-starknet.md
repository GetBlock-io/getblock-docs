---
description: >-
  Example code for the starknet_getBlockWithReceipts JSON-RPC method. Complete
  guide on how to use starknet_getBlockWithReceipts JSON-RPC in GetBlock Web3
  documentation.
---

# starknet\_getBlockWithReceipts - STRK

This method returns a block together with each transaction and its execution receipt, given a block\_id. It combines transaction bodies and outcomes in one response.

## Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| block\_id | object | string   | Yes         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getBlockWithReceipts",
    "params": ["latest"],
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
    method: 'starknet_getBlockWithReceipts',
    params: ['latest'],
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
        'method': 'starknet_getBlockWithReceipts',
        'params': ['latest'],
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
            "method": "starknet_getBlockWithReceipts",
            "params": ["latest"],
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
        "status": "ACCEPTED_ON_L2",
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "block_number": 700000,
        "transactions": [
            {
                "transaction": {
                    "transaction_hash": "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc",
                    "type": "INVOKE"
                },
                "receipt": {
                    "execution_status": "SUCCEEDED",
                    "finality_status": "ACCEPTED_ON_L2",
                    "actual_fee": {
                        "amount": "0x1ff973cafa7fff",
                        "unit": "WEI"
                    }
                }
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                    |
| --------- | ------ | -------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                              |
| id        | string | Request identifier matching the request                        |
| result    | object | Block with paired transactions and receipts (see fields below) |

### Result Object

| Field        | Type   | Description                           |
| ------------ | ------ | ------------------------------------- |
| transactions | array  | Array of {transaction, receipt} pairs |
| block\_hash  | string | Hash of the block                     |

## Use Cases

* **One-Call Indexing**: Fetch transactions and outcomes together
* **Fee Accounting**: Read actual\_fee per transaction in a block
* **Status Auditing**: Check execution\_status across a block
* **Explorer Backends**: Render blocks with outcomes inline

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 24 / BLOCK\_NOT\_FOUND    | Block not found | No block matches the supplied block\_id           |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
