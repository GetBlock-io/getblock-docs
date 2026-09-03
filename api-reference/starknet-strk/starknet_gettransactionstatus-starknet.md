# starknet\_getTransactionStatus starknet

This method returns the finality status (received, accepted on L2, accepted on L1) and execution status (succeeded, reverted) of a transaction by hash. It is the lightweight way to poll a transaction.

## Parameters

| Parameter         | Type   | Required | Description                     |
| ----------------- | ------ | -------- | ------------------------------- |
| transaction\_hash | string | Yes      | The transaction hash to look up |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getTransactionStatus",
    "params": ["0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc"],
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
    method: 'starknet_getTransactionStatus',
    params: ['0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc'],
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
        'method': 'starknet_getTransactionStatus',
        'params': ['0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc'],
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
            "method": "starknet_getTransactionStatus",
            "params": ["0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc"],
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
        "finality_status": "ACCEPTED_ON_L1",
        "execution_status": "SUCCEEDED"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | finality\_status and execution\_status  |

## Use Cases

* **Transaction Polling**: Poll status after submission until accepted
* **Confirmation Gating**: Wait for ACCEPTED\_ON\_L1 before treating a transfer as final
* **Revert Detection**: Detect REVERTED execution without fetching the full receipt
* **Wallet UX**: Show live transaction status in a signing UI

## Error Handling

| Error                      | Message                    | Description                                       |
| -------------------------- | -------------------------- | ------------------------------------------------- |
| 29 / TXN\_HASH\_NOT\_FOUND | Transaction hash not found | No transaction matches the supplied hash          |
| 403 / RBAC: access denied  | Access denied              | The GetBlock access token is missing or incorrect |
