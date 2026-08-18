---
description: >-
  Example code for the eth_getBlockByNumber JSON_RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON_RPC method in GetBlock Web3
  documentation.
---

# eth\_getBlockByNumber - Tron

This method returns information about a block by its number. The second parameter selects full transaction objects or transaction hashes.

## Parameters

| Parameter        | Type    | Required | Description                                         |
| ---------------- | ------- | -------- | --------------------------------------------------- |
| block            | string  | Yes      | Block number in hex, or "latest", "earliest"        |
| fullTransactions | boolean | Yes      | true for full transaction objects, false for hashes |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["0x40d2a00", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc', {
    jsonrpc: '2.0',
    method: 'eth_getBlockByNumber',
    params: ["0x40d2a00", false],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockByNumber',
        'params': ["0x40d2a00", false],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBlockByNumber",
            "params": ["0x40d2a00", false],
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
        "number": "0x40d2a00",
        "hash": "0x0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
        "parentHash": "0x0000000002f3a5af...",
        "timestamp": "0x6675a2c0",
        "transactions": [
            "0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"
        ],
        "gasLimit": "0x0",
        "gasUsed": "0x0",
        "size": "0x2a1f"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| id        | string | Request identifier matching the request           |
| result    | object | Block object, or null if the block does not exist |

### Result Object

| Field        | Type   | Description                                        |
| ------------ | ------ | -------------------------------------------------- |
| number       | string | Block number, hex-encoded                          |
| hash         | string | Block hash                                         |
| timestamp    | string | Block time in seconds, hex-encoded                 |
| transactions | array  | Transaction hashes, or full objects when requested |

## Use Cases

* **Block Reads**: Read a block through Ethereum-style tooling
* **Indexing**: Stream blocks by number into a store
* **Timestamp Mapping**: Read a block's time for time-based logic
* **Transaction Lists**: Read the transactions in a block

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
