---
description: >-
  Example code for the starknet_getBlockWithTxs JSON-RPC method. Complete guide
  on how to use starknet_getBlockWithTxs JSON-RPC in GetBlock Web3
  documentation.
---

# starknet\_getBlockWithTxs - STRK

This method returns a block's header together with the full body of every transaction it contains, given a block\_id. It is used when transaction contents are needed alongside block metadata.

## Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| block\_id | object | string   | Yes         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getBlockWithTxs",
    "params": [{ "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4" }],
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
    method: 'starknet_getBlockWithTxs',
    params: [{ block_hash: '0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4' }],
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
        'method': 'starknet_getBlockWithTxs',
        'params': [{ 'block_hash': '0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4' }],
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
            "method": "starknet_getBlockWithTxs",
            "params": [{ "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4" }],
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
        "status": "ACCEPTED_ON_L1",
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "block_number": 700000,
        "timestamp": 1710000000,
        "sequencer_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
        "starknet_version": "0.13.2",
        "transactions": [
            {
                "transaction_hash": "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc",
                "type": "INVOKE",
                "version": "0x1",
                "nonce": "0x2a",
                "sender_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
                "calldata": [
                    "0x1",
                    "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7",
                    "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e",
                    "0x1",
                    "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b"
                ]
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | object | Block header plus full transactions (see fields below) |

### Result Object

| Field              | Type   | Description                                                   |
| ------------------ | ------ | ------------------------------------------------------------- |
| block\_hash        | string | Hash of the block                                             |
| transactions       | array  | Full transaction objects (type, version, calldata, signature) |
| sequencer\_address | string | Address of the sequencer that produced the block              |

## Use Cases

* **Transaction Indexing**: Ingest full transaction bodies per block
* **Calldata Decoding**: Decode INVOKE calldata into contract calls
* **Explorer Backends**: Render block detail pages with transactions
* **Analytics**: Aggregate transaction types per block

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 24 / BLOCK\_NOT\_FOUND    | Block not found | No block matches the supplied block\_id           |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
