---
description: >-
  Example code for the iotax_getCurrentEpoch JSON-RPC method. Complete guide on
  how to use iotax_getCurrentEpoch JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_getCurrentEpoch - IOTA

Returns information about the current epoch: its number, validators, start time, and reference gas price. Served by indexer-backed nodes.

## Parameters

This method takes no parameters.

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
    "method": "iotax_getCurrentEpoch",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getCurrentEpoch', params: [] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getCurrentEpoch', 'params': []})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getCurrentEpoch","params":[]})).send().await?.json::<Value>().await?;
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
    "result": {
        "epoch": "742",
        "validators": [],
        "epochTotalTransactions": "1200000",
        "firstCheckpointId": "128000000",
        "epochStartTimestampMs": "1736700000000",
        "referenceGasPrice": "1000"
    }
}
```

## Response Parameters

| Field                        | Type   | Description                        |
| ---------------------------- | ------ | ---------------------------------- |
| jsonrpc                      | string | JSON-RPC version ("2.0")           |
| id                           | string | Request identifier ("getblock.io") |
| result.epoch                 | string | Current epoch number               |
| result.referenceGasPrice     | string | Reference gas price for the epoch  |
| result.epochStartTimestampMs | string | Epoch start time                   |

## Use Cases

* **Epoch Tracking**: Follow epoch transitions
* **Gas**: Read the epoch reference gas price

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
