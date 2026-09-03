# starknet\_addInvokeTransaction - STRK

This method submits a signed INVOKE transaction — an account calling one or more contracts — to the sequencer and returns its transaction hash. It is the primary write path for dApps.

## Parameters

| Parameter           | Type   | Required | Description                                         |
| ------------------- | ------ | -------- | --------------------------------------------------- |
| invoke\_transaction | object | Yes      | A signed INVOKE transaction (version 3 recommended) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_addInvokeTransaction",
    "params": [{ "type": "INVOKE", "version": "0x3", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "calldata": ["0x1", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "0x1", "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"], "signature": ["0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f"], "resource_bounds": { "l1_gas": { "max_amount": "0x1a4", "max_price_per_unit": "0x3b9aca00" }, "l2_gas": { "max_amount": "0x0", "max_price_per_unit": "0x0" } } }],
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
    method: 'starknet_addInvokeTransaction',
    params: [{ type: 'INVOKE', version: '0x3', sender_address: '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', nonce: '0x2b', calldata: ['0x1', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', '0x1', '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'], signature: ['0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f'], resource_bounds: { l1_gas: { max_amount: '0x1a4', max_price_per_unit: '0x3b9aca00' }, l2_gas: { max_amount: '0x0', max_price_per_unit: '0x0' } } }],
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
        'method': 'starknet_addInvokeTransaction',
        'params': [{ 'type': 'INVOKE', 'version': '0x3', 'sender_address': '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', 'nonce': '0x2b', 'calldata': ['0x1', '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', '0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e', '0x1', '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b'], 'signature': ['0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f'], 'resource_bounds': { 'l1_gas': { 'max_amount': '0x1a4', 'max_price_per_unit': '0x3b9aca00' }, 'l2_gas': { 'max_amount': '0x0', 'max_price_per_unit': '0x0' } } }],
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
            "method": "starknet_addInvokeTransaction",
            "params": [{ "type": "INVOKE", "version": "0x3", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "calldata": ["0x1", "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e", "0x1", "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"], "signature": ["0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f"], "resource_bounds": { "l1_gas": { "max_amount": "0x1a4", "max_price_per_unit": "0x3b9aca00" }, "l2_gas": { "max_amount": "0x0", "max_price_per_unit": "0x0" } } }],
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
        "transaction_hash": "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                 |
| --------- | ------ | ------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")           |
| id        | string | Request identifier matching the request     |
| result    | object | Object with the submitted transaction\_hash |

## Use Cases

* **Contract Calls**: Submit state-changing calls from an account
* **Token Transfers**: Broadcast ERC-20 transfers
* **Multicall**: Pack several calls into one INVOKE
* **dApp Writes**: Send user-signed transactions to the network

## Error Handling

| Error                            | Message                   | Description                                          |
| -------------------------------- | ------------------------- | ---------------------------------------------------- |
| 55 / ACCOUNT\_VALIDATION\_FAILED | Account validation failed | The account's **validate** rejected the transaction  |
| 52 / INVALID\_TRANSACTION\_NONCE | Invalid nonce             | The nonce does not match the account's current nonce |
