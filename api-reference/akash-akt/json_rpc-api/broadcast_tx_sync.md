# broadcast\_tx\_sync

Submits a signed transaction and returns after CheckTx (mempool validation), without waiting for a block. Returns the transaction hash and CheckTx code.

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
    "method": "broadcast_tx_sync",
    "params": {"tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'broadcast_tx_sync', params: {"tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'broadcast_tx_sync', 'params': {"tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"broadcast_tx_sync","params":{"tx": "Cr0BCroBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5k..."}})).send().await?.json::<Value>().await?;
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
        "hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C",
        "log": "[]"
    }
}
```

## Response Fields

| Field | Type    | Description                         |
| ----- | ------- | ----------------------------------- |
| hash  | string  | Tx hash to track with the tx method |
| code  | integer | CheckTx code; 0 = accepted          |
| log   | string  | CheckTx log on rejection            |

## Use Cases

* **Submission**: Broadcast a signed Akash transaction
* **Throughput**: Submit then poll status
* **Validation**: Detect rejections early

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Undecodable transaction                           |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
