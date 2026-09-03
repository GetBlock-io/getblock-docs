# starknet\_getTransactionByBlockIdAndIndex starknet

This method returns the transaction at a given index within the block identified by block\_id. It is used to iterate a block's transactions positionally.

## Parameters

| Parameter | Type    | Required | Description                                      |
| --------- | ------- | -------- | ------------------------------------------------ |
| block\_id | object  | string   | Yes                                              |
| index     | integer | Yes      | Zero-based index of the transaction in the block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getTransactionByBlockIdAndIndex",
    "params": { "block_id": { "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4" }, "index": 1 },
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
    method: 'starknet_getTransactionByBlockIdAndIndex',
    params: { block_id: { block_hash: '0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4' }, index: 1 },
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
        'method': 'starknet_getTransactionByBlockIdAndIndex',
        'params': { 'block_id': { 'block_hash': '0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4' }, 'index': 1 },
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
            "method": "starknet_getTransactionByBlockIdAndIndex",
            "params": { "block_id": { "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4" }, "index": 1 },
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
        "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
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
| result    | object | Transaction body at the requested index |

## Use Cases

* **Positional Iteration**: Walk a block's transactions by index
* **Indexing**: Pair transactions with their block position
* **Explorer Backends**: Resolve a transaction from a block + index route
* **Verification**: Cross-check a transaction's placement in a block

## Error Handling

| Error                    | Message                   | Description                             |
| ------------------------ | ------------------------- | --------------------------------------- |
| 27 / INVALID\_TXN\_INDEX | Invalid transaction index | The index is out of range for the block |
| 24 / BLOCK\_NOT\_FOUND   | Block not found           | No block matches the supplied block\_id |
