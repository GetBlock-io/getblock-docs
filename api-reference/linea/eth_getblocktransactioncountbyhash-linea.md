---
description: >-
  Example code for the eth_getBlockTransactionCountByHash JSON-RPC method.
  Complete guide on how to use eth_getBlockTransactionCountByHash JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getBlockTransactionCountByHash  - Linea

This method returns the number of transactions in a block identified by its hash.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| blockHash | string | Yes      | 32-byte block hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f"],
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
    method: 'eth_getBlockTransactionCountByHash',
    params: ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f"],
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
        'method': 'eth_getBlockTransactionCountByHash',
        'params': ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f"],
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
            "method": "eth_getBlockTransactionCountByHash",
            "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f"],
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
    "result": "0x2a"
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | string | Hex-encoded count of transactions in the block |

## Use Cases

* **Block Analytics**: Count transactions in a block found by hash
* **Pagination**: Size iteration over a block's transactions
* **Verification**: Confirm the expected transaction count for a block
* **Explorer Headers**: Show the transaction count on a block page

## Error Handling

| Error Code | Message        | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| -32602     | Invalid params | The block number or hash is malformed or out of range |
| -32603     | Internal error | The node failed to read the block                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const count = await provider.send('eth_getBlockTransactionCountByHash', ['0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f']);
console.log(parseInt(count, 16));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const count = await client.getBlockTransactionCount({ blockHash: '0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f' });
console.log(count);
```
{% endcode %}
{% endtab %}
{% endtabs %}
