---
description: >-
  Example code for the broadcast_tx_commit JSON-RPC method. Complete guide on
  how to use broadcast_tx_commit JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_commit - Akash

Submits a signed transaction and waits for it to be included in a block, returning both the CheckTx and DeliverTx (execution) results. Slower; not recommended for high load.

## Parameters

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| tx        | string | Yes      | Base64 signed transaction bytes |

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
    "method": "broadcast_tx_commit",
    "params": {"tx": "Cr0BC..."}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'broadcast_tx_commit', params: {"tx": "Cr0BC..."} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'broadcast_tx_commit', 'params': {"tx": "Cr0BC..."}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"broadcast_tx_commit","params":{"tx": "Cr0BC..."}})).send().await?.json::<Value>().await?;
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
        "hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C",
        "height": "19500000",
        "check_tx": {
            "code": 0
        },
        "tx_result": {
            "code": 0,
            "gas_used": "118000"
        }
    }
}
```

## Response Fields

| Field      | Type   | Description                |
| ---------- | ------ | -------------------------- |
| hash       | string | Transaction hash           |
| height     | string | Block height it landed in  |
| tx\_result | object | DeliverTx execution result |

## Use Cases

* **Synchronous Submission**: Submit and get the outcome in one call
* **Simple Scripts**: Await inclusion without polling

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Timed out or undecodable                          |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
