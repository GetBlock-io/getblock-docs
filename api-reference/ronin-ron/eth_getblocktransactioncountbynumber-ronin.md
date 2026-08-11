---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber JSON-RPC method.
  Complete guide to using the eth_getBlockTransactionCountByNumber JSON-RPC
  method in the GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Ronin

This method returns the number of transactions in a block identified by its number.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": ["0x1e8480"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockTransactionCountByNumber',
    params: ["0x1e8480"],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockTransactionCountByNumber',
        'params': ["0x1e8480"],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBlockTransactionCountByNumber",
            "params": ["0x1e8480"],
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

* **Block Analytics**: Track transactions per block over time
* **Pagination**: Size iteration over a block's transactions
* **Throughput Metrics**: Measure block fullness by transaction count
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

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const count = await provider.send('eth_getBlockTransactionCountByNumber', ['0x1e8480']);
console.log(parseInt(count, 16));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { ronin } from 'viem/chains';

const client = createPublicClient({ chain: ronin, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const count = await client.getBlockTransactionCount({ blockNumber: 2000000n });
console.log(count);
```
{% endcode %}
{% endtab %}
{% endtabs %}
