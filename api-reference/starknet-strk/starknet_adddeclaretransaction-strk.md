# starknet\_addDeclareTransaction -STRK

This method submits a signed DECLARE transaction, registering a new contract class on the network so it can be deployed, and returns the transaction hash and class hash.

## Parameters

| Parameter            | Type   | Required | Description                                          |
| -------------------- | ------ | -------- | ---------------------------------------------------- |
| declare\_transaction | object | Yes      | A signed DECLARE transaction with the contract class |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_addDeclareTransaction",
    "params": [{ "type": "DECLARE", "version": "0x3", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "compiled_class_hash": "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003", "contract_class": { "sierra_program": ["0x1"], "abi": "[]" }, "signature": ["0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f"], "resource_bounds": { "l1_gas": { "max_amount": "0x1a4", "max_price_per_unit": "0x3b9aca00" }, "l2_gas": { "max_amount": "0x0", "max_price_per_unit": "0x0" } } }],
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
    method: 'starknet_addDeclareTransaction',
    params: [{ type: 'DECLARE', version: '0x3', sender_address: '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', nonce: '0x2b', compiled_class_hash: '0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003', contract_class: { sierra_program: ['0x1'], abi: '[]' }, signature: ['0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f'], resource_bounds: { l1_gas: { max_amount: '0x1a4', max_price_per_unit: '0x3b9aca00' }, l2_gas: { max_amount: '0x0', max_price_per_unit: '0x0' } } }],
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
        'method': 'starknet_addDeclareTransaction',
        'params': [{ 'type': 'DECLARE', 'version': '0x3', 'sender_address': '0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b', 'nonce': '0x2b', 'compiled_class_hash': '0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003', 'contract_class': { 'sierra_program': ['0x1'], 'abi': '[]' }, 'signature': ['0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f'], 'resource_bounds': { 'l1_gas': { 'max_amount': '0x1a4', 'max_price_per_unit': '0x3b9aca00' }, 'l2_gas': { 'max_amount': '0x0', 'max_price_per_unit': '0x0' } } }],
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
            "method": "starknet_addDeclareTransaction",
            "params": [{ "type": "DECLARE", "version": "0x3", "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b", "nonce": "0x2b", "compiled_class_hash": "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003", "contract_class": { "sierra_program": ["0x1"], "abi": "[]" }, "signature": ["0x5a1c0af2b96c461a9753e383107e2bba1849cdf6029ffaa2b97533ada03789f"], "resource_bounds": { "l1_gas": { "max_amount": "0x1a4", "max_price_per_unit": "0x3b9aca00" }, "l2_gas": { "max_amount": "0x0", "max_price_per_unit": "0x0" } } }],
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
        "class_hash": "0x01a736d6ed154502257f02b1ccdf4d9d1089f80811cd6acad48e6b6a9d1f2003"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | object | transaction\_hash and the declared class\_hash |

## Use Cases

* **Contract Publishing**: Declare a class before deploying instances
* **Upgrades**: Declare a new implementation class
* **Tooling**: Register classes as part of a deploy pipeline
* **Verification**: Confirm the returned class hash matches the build

## Error Handling

| Error                         | Message                | Description                                  |
| ----------------------------- | ---------------------- | -------------------------------------------- |
| 51 / CLASS\_ALREADY\_DECLARED | Class already declared | A class with this hash is already registered |
| 56 / COMPILATION\_FAILED      | Compilation failed     | The contract class failed to compile         |
