---
description: >-
  Example code for the zks_getRawBlockTransactions JSON-RPC method. Complete
  guide on how to use zks_getRawBlockTransactions JSON-RPC in GetBlock Web3
  documentation.
---

# zks\_getRawBlockTransactions - Abstract

Returns the raw, ZK-native transaction objects for an L2 block, including the common data and execution fields used by the ZK Stack.

## Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| block     | integer | Yes      | L2 block number |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getRawBlockTransactions",
    "params": [10000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getRawBlockTransactions', params: [10000], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getRawBlockTransactions', 'params': [10000], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getRawBlockTransactions","params":[10000],"id":"getblock.io"})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
        {
            "common_data": {
                "L2": {
                    "nonce": 42,
                    "fee": {
                        "gas_limit": "0x5208"
                    },
                    "initiatorAddress": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
                }
            },
            "execute": {
                "contractAddress": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                "calldata": "0x..."
            }
        }
    ]
}
```

## Response Fields

| Field  | Type  | Description                                              |
| ------ | ----- | -------------------------------------------------------- |
| result | array | Raw ZK transactions with common\_data and execute fields |

## Use Cases

* **Indexing**: Ingest ZK-native transaction data
* **Debugging**: Inspect raw transaction structure

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
