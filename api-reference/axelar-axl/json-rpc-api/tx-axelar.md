---
description: >-
  Example code for the tx JSON-RPC method. Complete guide on how to use tx
  JSON-RPC in GetBlock Web3 documentation.
---

# tx - Axelar

Returns the execution result of a transaction by hash: height, gas, ABCI code, and events, optionally with a proof.

## Parameters

| Parameter | Type    | Required | Description            |
| --------- | ------- | -------- | ---------------------- |
| hash      | string  | Yes      | Transaction hash (hex) |
| prove     | boolean | Optional | Include a Merkle proof |

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
    "method": "tx",
    "params": {"hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C", "prove": false}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'tx', params: {"hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C", "prove": false} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'tx', 'params': {"hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C", "prove": false}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"tx","params":{"hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C", "prove": false}})).send().await?.json::<Value>().await?;
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
        "tx_result": {
            "code": 0,
            "gas_used": "118000",
            "events": []
        },
        "tx": "Cr0BC..."
    }
}
```

## Response Fields

| Field      | Type   | Description                    |
| ---------- | ------ | ------------------------------ |
| height     | string | Block height the tx landed in  |
| tx\_result | object | ABCI result: code, gas, events |
| tx         | string | Base64 raw transaction         |

## Use Cases

* **Status Checks**: Confirm a tx succeeded
* **Receipts**: Extract events
* **Explorers**: Render tx pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Not found     | No tx matches the hash                            |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
