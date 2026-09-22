---
description: >-
  Example code for the broadcast_tx_async JSON-RPC method. Complete guide on
  how to use broadcast_tx_async JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_async - Akash

Submits a signed transaction and returns immediately with its hash, without waiting for CheckTx or a block.

{% hint style="info" %}
**The transaction below is a placeholder and cannot be broadcast as written.** Sent unchanged it
returns `-32602 Invalid params` with an `illegal base64` message. Substitute a real signed,
protobuf-encoded transaction, base64-encoded. The response shown is the shape returned on
acceptance.
{% endhint %}

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
    "method": "broadcast_tx_async",
    "params": {"tx": "Cr0BC..."}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'broadcast_tx_async', params: {"tx": "Cr0BC..."} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'broadcast_tx_async', 'params': {"tx": "Cr0BC..."}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"broadcast_tx_async","params":{"tx": "Cr0BC..."}})).send().await?.json::<Value>().await?;
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
        "code": 0,
        "hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C"
    }
}
```

## Response Fields

| Field | Type    | Description                   |
| ----- | ------- | ----------------------------- |
| hash  | string  | Transaction hash              |
| code  | integer | Always 0 for async submission |

## Use Cases

* **High Throughput**: Submit many transactions fast
* **Fire-and-Forget**: Broadcast then poll status

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Undecodable transaction                           |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
