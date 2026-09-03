# starknet\_getTransactionReceipt starknet

This method returns the execution receipt of a transaction: its status, actual fee, emitted events, and L2-to-L1 messages. It is used to confirm outcomes and read events.

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
    "method": "starknet_getTransactionReceipt",
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
    method: 'starknet_getTransactionReceipt',
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
        'method': 'starknet_getTransactionReceipt',
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
            "method": "starknet_getTransactionReceipt",
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
        "transaction_hash": "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc",
        "execution_status": "SUCCEEDED",
        "finality_status": "ACCEPTED_ON_L1",
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "block_number": 700000,
        "actual_fee": {
            "amount": "0x1ff973cafa7fff",
            "unit": "WEI"
        },
        "events": [
            {
                "from_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7",
                "keys": [
                    "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e"
                ],
                "data": [
                    "0x2a"
                ]
            }
        ],
        "messages_sent": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Transaction receipt (see fields below)  |

### Result Object

| Field             | Type   | Description                                      |
| ----------------- | ------ | ------------------------------------------------ |
| execution\_status | string | SUCCEEDED or REVERTED                            |
| actual\_fee       | object | Fee actually charged, with its unit (WEI or FRI) |
| events            | array  | Events emitted during execution                  |
| messages\_sent    | array  | L2-to-L1 messages produced by the transaction    |

## Use Cases

* **Outcome Confirmation**: Confirm a transaction succeeded and read its fee
* **Event Reading**: Extract emitted events for indexing
* **Bridge Flows**: Read L2-to-L1 messages for withdrawals
* **Fee Accounting**: Record the actual fee paid

## Error Handling

| Error                      | Message                    | Description                                       |
| -------------------------- | -------------------------- | ------------------------------------------------- |
| 29 / TXN\_HASH\_NOT\_FOUND | Transaction hash not found | No transaction matches the supplied hash          |
| 403 / RBAC: access denied  | Access denied              | The GetBlock access token is missing or incorrect |
