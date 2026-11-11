---
description: >-
  Example code for the eth_getTransactionReceipt JSON_RPC method. Complete guide
  on how to use eth_getTransactionReceipt JSON_RPC method in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt - Tron

This method returns the receipt of a transaction by its hash, including status and logs, in the Ethereum-compatible shape.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/jsonrpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
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
    method: 'eth_getTransactionReceipt',
    params: ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
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
        'method': 'eth_getTransactionReceipt',
        'params': ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
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
            "method": "eth_getTransactionReceipt",
            "params": ["0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62"],
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
        "transactionHash": "0xd5ec749ecc2a615399d8a6c864ea4c74ff9f523c2be0e341ac9be5d47d7c2d62",
        "blockHash": "0x0000000002f3a5b0f6d2e6c9a1b4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8",
        "blockNumber": "0x40d2a00",
        "from": "0x41f0cc5a2a84cd0f68ed1667070934542d673acbd8",
        "to": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "status": "0x1",
        "gasUsed": "0xfde8",
        "logs": [
            {
                "address": "0x41a614f803b6fd780986a42c78ec9c7f77e6ded13c",
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                ],
                "data": "0x..."
            }
        ],
        "transactionIndex": "0x0"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| id        | string | Request identifier matching the request                   |
| result    | object | Transaction receipt object, or null if pending or unknown |

### Result Object

| Field           | Type   | Description                                 |
| --------------- | ------ | ------------------------------------------- |
| status          | string | 0x1 on success, 0x0 on failure              |
| gasUsed         | string | Energy consumed, expressed in the gas field |
| logs            | array  | Event logs emitted by the transaction       |
| transactionHash | string | Hash of the transaction                     |

## Use Cases

* **Confirmation Checks**: Confirm a transaction succeeded by its status
* **Event Reads**: Read TRC-20 logs through Ethereum-style tooling
* **Receipt Pipelines**: Ingest receipts into an EVM-oriented indexer
* **Energy Accounting**: Read the gas field for Energy used

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |
