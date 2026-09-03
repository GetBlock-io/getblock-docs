# starknet\_getTransactionByHash starknet

This method returns the full body of a transaction identified by its hash, including its type, version, signature, and calldata.

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
    "method": "starknet_getTransactionByHash",
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
    method: 'starknet_getTransactionByHash',
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
        'method': 'starknet_getTransactionByHash',
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
            "method": "starknet_getTransactionByHash",
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
        "type": "INVOKE",
        "version": "0x1",
        "nonce": "0x0",
        "max_fee": "0x1ff973cafa7fff",
        "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
        "signature": [
            "0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f"
        ],
        "calldata": [
            "0x1",
            "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7",
            "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e",
            "0x1",
            "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Transaction body (see fields below)     |

### Result Object

| Field           | Type   | Description                                                      |
| --------------- | ------ | ---------------------------------------------------------------- |
| type            | string | Transaction type (INVOKE, DECLARE, DEPLOY\_ACCOUNT, L1\_HANDLER) |
| sender\_address | string | Account that sent the transaction                                |
| calldata        | array  | Encoded call arguments as felts                                  |
| signature       | array  | Account signature over the transaction hash                      |

## Use Cases

* **Transaction Lookup**: Fetch a transaction body by hash
* **Calldata Decoding**: Decode the calls packed into an INVOKE
* **Wallet History**: Populate transaction detail views
* **Debugging**: Inspect a submitted transaction's fields

## Error Handling

| Error                      | Message                    | Description                                       |
| -------------------------- | -------------------------- | ------------------------------------------------- |
| 29 / TXN\_HASH\_NOT\_FOUND | Transaction hash not found | No transaction matches the supplied hash          |
| 403 / RBAC: access denied  | Access denied              | The GetBlock access token is missing or incorrect |
