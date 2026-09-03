---
description: >-
  Example code for the starknet_getEvents JSON-RPC method. Complete guide on how
  to use starknet_getEvents JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_getEvents - STRK

This method returns events matching a filter over a block range, address, and key set, paginated by a chunk size and continuation token. It is the primary way to read logs on Starknet.

## Parameters

| Parameter | Type   | Required | Description                                                             |
| --------- | ------ | -------- | ----------------------------------------------------------------------- |
| filter    | object | Yes      | from\_block, to\_block, address, keys, chunk\_size, continuation\_token |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getEvents",
    "params": [{ "from_block": { "block_number": 700000 }, "to_block": "latest", "address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "keys": [["0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e"]], "chunk_size": 100 }],
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
    method: 'starknet_getEvents',
    params: [{ from_block: { block_number: 700000 }, to_block: 'latest', address: '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', keys: [['0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e']], chunk_size: 100 }],
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
        'method': 'starknet_getEvents',
        'params': [{ 'from_block': { 'block_number': 700000 }, 'to_block': 'latest', 'address': '0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7', 'keys': [['0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e']], 'chunk_size': 100 }],
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
            "method": "starknet_getEvents",
            "params": [{ "from_block": { "block_number": 700000 }, "to_block": "latest", "address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7", "keys": [["0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e"]], "chunk_size": 100 }],
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
        "events": [
            {
                "from_address": "0x049d36570d4e46f48e99674bd3fcc84644ddd6b96f7c741b1562b82f9e004dc7",
                "keys": [
                    "0x2e4263afad30923c891518314c3c95dbe830a16874e8abc5777a9a20b54c76e"
                ],
                "data": [
                    "0x2a"
                ],
                "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
                "block_number": 700000,
                "transaction_hash": "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc"
            }
        ],
        "continuation_token": "1234"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                 |
| --------- | ------ | ----------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                           |
| id        | string | Request identifier matching the request                     |
| result    | object | Matching events and a continuation token (see fields below) |

### Result Object

| Field               | Type   | Description                                                    |
| ------------------- | ------ | -------------------------------------------------------------- |
| events              | array  | Events matching the filter, with block and transaction context |
| continuation\_token | string | Token to fetch the next page, absent on the last page          |

## Use Cases

* **Log Indexing**: Ingest contract events into an off-chain index
* **Transfer Tracking**: Filter Transfer events for a token
* **Pagination**: Page large result sets with the continuation token
* **Analytics**: Aggregate event activity over a block range

## Error Handling

| Error                             | Message                    | Description                                    |
| --------------------------------- | -------------------------- | ---------------------------------------------- |
| 33 / INVALID\_CONTINUATION\_TOKEN | Invalid continuation token | The continuation token is malformed or expired |
| 34 / TOO\_MANY\_KEYS\_IN\_FILTER  | Too many keys in filter    | The filter contains more keys than allowed     |
