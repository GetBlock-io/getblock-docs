---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex JSON-RPC method.
  Complete guide on how to use eth_getTransactionByBlockNumberAndIndex JSON-RPC
  in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Harmony

This method returns the transaction at a given index within a block identified by its number or tag.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |
| index          | string | Yes      | Transaction index within the block (hex)                |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": ["latest", "0x0"],
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
    method: 'eth_getTransactionByBlockNumberAndIndex',
    params: ['latest', '0x0'],
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
        'method': 'eth_getTransactionByBlockNumberAndIndex',
        'params': ['latest', '0x0'],
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
            "method": "eth_getTransactionByBlockNumberAndIndex",
            "params": ["latest", "0x0"],
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
        "hash": "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234",
        "blockHash": "0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d",
        "blockNumber": "0x3d09000",
        "transactionIndex": "0x0",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xcF664087a5bB0237a0BAd6742852ec6c8d69A27a",
        "value": "0x0"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                        |
| id        | string | Request identifier matching the request                  |
| result    | object | Transaction object, or null if the index is out of range |

## Use Cases

* **Positional Lookup**: Fetch a transaction by height and index
* **Live Block Scanning**: Read the first transaction of the "latest" block
* **Block Iteration**: Walk a block's transactions by index without its hash
* **Explorer Backends**: Resolve a transaction from a number-and-index reference

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32602     | Invalid params | Malformed block number/tag or index     |
| -32603     | Internal error | The node failed to read the transaction |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.send('eth_getTransactionByBlockNumberAndIndex', ['latest', '0x0']);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const tx = await client.getTransaction({ blockTag: 'latest', index: 0 });
```
{% endcode %}
{% endtab %}
{% endtabs %}
