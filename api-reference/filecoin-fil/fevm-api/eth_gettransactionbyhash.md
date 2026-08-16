---
description: >-
  Example code for the eth_getTransactionByHash JSON RPC method. Complete guide
  on how to use eth_getTransactionByHash JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Filecoin

This method returns information about a transaction identified by its hash, including its sender, recipient, value, and inclusion data.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"],
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
    method: 'eth_getTransactionByHash',
    params: ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"],
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
        'method': 'eth_getTransactionByHash',
        'params': ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"],
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
            "method": "eth_getTransactionByHash",
            "params": ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"],
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
        "hash": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b",
        "blockHash": "0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f",
        "blockNumber": "0x1e8480",
        "transactionIndex": "0x0",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0x60E1773636CF5E4A227d9AC24F20fEca034ee25A",
        "value": "0xde0b6b3a7640000",
        "gas": "0x5208",
        "gasPrice": "0x3b9aca00",
        "nonce": "0x2a",
        "input": "0x",
        "type": "0x2",
        "chainId": "0x13a"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")        |
| id        | string | Request identifier matching the request  |
| result    | object | Transaction object, or null if not found |

### Result Object

| Field       | Type   | Description                                      |
| ----------- | ------ | ------------------------------------------------ |
| hash        | string | Transaction hash                                 |
| blockNumber | string | Block number of inclusion, or null if pending    |
| from        | string | Sender address                                   |
| to          | string | Recipient address, or null for contract creation |
| value       | string | Value transferred in wei                         |
| nonce       | string | Sender nonce for the transaction                 |

## Use Cases

* **Transaction Views**: Display a transaction's details in a wallet or explorer
* **Status Tracking**: Check whether a transaction has been included in a block
* **Value Reads**: Read the amount and recipient of a transfer
* **Debugging**: Inspect a transaction's input data and parameters

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | The transaction hash is not a valid 32-byte hex string |
| -32603     | Internal error | The node failed to read the transaction                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.getTransaction('0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b');
console.log(tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { filecoin } from 'viem/chains';

const client = createPublicClient({ chain: filecoin, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const tx = await client.getTransaction({ hash: '0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b' });
console.log(tx);
```
{% endcode %}
{% endtab %}
{% endtabs %}
