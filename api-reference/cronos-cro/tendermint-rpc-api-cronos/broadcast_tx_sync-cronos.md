---
description: >-
  Example code for the broadcast_tx_sync JSON-RPC method. Complete guide on how
  to use broadcast_tx_sync JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_sync - Cronos

Submits a signed transaction and returns once CheckTx has run, without waiting for the transaction to be included in a block. A `code` of 0 means the transaction was accepted into the mempool, not that it succeeded on chain.

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| tx        | string | Yes      | Base64-encoded signed transaction bytes |

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
    "method": "broadcast_tx_sync",
    "params": {
        "tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."
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
    method: 'broadcast_tx_sync',
    params: {
        "tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."
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
        'method': 'broadcast_tx_sync',
        'params': {
            "tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."
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
            "method": "broadcast_tx_sync",
            "params": {
                "tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "code": 0,
        "data": "",
        "log": "",
        "codespace": "",
        "hash": "A1B2C3D4E5F60718293A4B5C6D7E8F90112233445566778899AABBCCDDEEFF00"
    }
}
```
{% endcode %}

## Response Fields

| Field     | Type    | Description                                                         |
| --------- | ------- | ------------------------------------------------------------------- |
| code      | integer | CheckTx ABCI code: 0 if accepted into the mempool                   |
| data      | string  | Base64 response data from CheckTx (usually empty)                   |
| log       | string  | Log message; carries the reason when the transaction is rejected    |
| codespace | string  | Module codespace identifying the source of a non-zero code          |
| hash      | string  | Transaction hash — poll `tx` or `tx_search` to confirm inclusion    |

## Use Cases

* **Transaction Submission**: Broadcast a signed transaction and get the hash back
* **Fast Rejection**: Catch signature, sequence, and fee errors via a non-zero code
* **Wallet Backends**: Submit, then confirm inclusion with a follow-up tx lookup

{% hint style="info" %}
A zero `code` only confirms the transaction passed CheckTx and entered the mempool. Confirm on-chain execution with `tx` or `tx_search` using the returned hash.
{% endhint %}

## Error Handling

| Error                     | Message        | Description                                                     |
| ------------------------- | -------------- | --------------------------------------------------------------- |
| -32603 / Internal error   | Internal error | The tx field is not valid base64 or the transaction is malformed |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
