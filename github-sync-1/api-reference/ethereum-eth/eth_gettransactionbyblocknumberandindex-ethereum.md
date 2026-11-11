---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex JSON RPC
  in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Ethereum

This method returns transaction data by block number and transaction index within that block. Same output as `eth_getTransactionByBlockHashAndIndex`, but with block-number-based lookup — the convenient path for indexers iterating over historical blocks.

## Parameters

| Parameter          | Type   | Required | Description                                                                  |
| ------------------ | ------ | -------- | ---------------------------------------------------------------------------- |
| `blockParameter`   | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |
| `transactionIndex` | string | Yes      | Hex-encoded transaction index within the block (0-based)                     |

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
    "params": [
        "latest",
        "0x0"
    ],
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
    params: [
        "latest",
        "0x0"
    ],
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
        'params': [
        "latest",
        "0x0"
    ],
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
            "params": [
        "latest",
        "0x0"
    ],
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
        "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "nonce": "0x462",
        "blockHash": "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        "blockNumber": "0x1720340",
        "transactionIndex": "0x0",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "value": "0x0",
        "gas": "0x186a0",
        "gasPrice": "0x9502f900",
        "maxFeePerGas": "0xb2d05e00",
        "maxPriorityFeePerGas": "0x3b9aca00",
        "input": "0xa9059cbb000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb480000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "v": "0x0",
        "r": "0xa3b4c5d6e7f8091a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a",
        "s": "0xb6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7",
        "chainId": "0x1",
        "type": "0x2",
        "accessList": [],
        "yParity": "0x0"
    }
}
```

## Response Parameters

| Parameter                     | Type            | Description                                                                                                  |
| ----------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------ |
| `jsonrpc`                     | string          | JSON-RPC protocol version ("2.0")                                                                            |
| `id`                          | string          | Request identifier matching the request                                                                      |
| `result.hash`                 | string          | Transaction hash                                                                                             |
| `result.nonce`                | string          | Nonce of the sender at the time of signing                                                                   |
| `result.blockHash`            | string          | Hash of the block containing this transaction (`null` if pending)                                            |
| `result.blockNumber`          | string          | Block number containing this transaction (`null` if pending)                                                 |
| `result.transactionIndex`     | string          | Position of this transaction within its block (`null` if pending)                                            |
| `result.from`                 | string          | Sender address                                                                                               |
| `result.to`                   | string          | Recipient address (`null` for contract creation)                                                             |
| `result.value`                | string          | Value transferred (wei, hex)                                                                                 |
| `result.gas`                  | string          | Gas limit for this transaction                                                                               |
| `result.gasPrice`             | string          | Effective gas price (wei, hex)                                                                               |
| `result.maxFeePerGas`         | string          | EIP-1559 maximum total fee per gas (type-2 transactions)                                                     |
| `result.maxPriorityFeePerGas` | string          | EIP-1559 priority fee per gas (type-2 transactions)                                                          |
| `result.input`                | string          | Encoded call data                                                                                            |
| `result.v`                    | string          | Signature v value                                                                                            |
| `result.r`                    | string          | Signature r value                                                                                            |
| `result.s`                    | string          | Signature s value                                                                                            |
| `result.chainId`              | string          | Chain ID the transaction is bound to (EIP-155)                                                               |
| `result.type`                 | string          | Transaction type (`0x0` legacy, `0x1` EIP-2930, `0x2` EIP-1559, `0x3` EIP-4844 blob, `0x4` EIP-7702 SetCode) |
| `result.accessList`           | array of object | EIP-2930 access list (type-1+ transactions)                                                                  |
| `result.yParity`              | string          | Signature parity (post-London)                                                                               |

## Use Cases

* **Historical Backfill**: Iterate `(blockNumber, index)` pairs to backfill transaction data into an indexer database
* **Live Transaction Feed**: Combine `eth_blockNumber` polling with per-block transaction iteration for a live feed
* **Positional Studies**: Fetch a specific index across many blocks (e.g. always position 0) for MEV analysis

## Error Handling

| Error Code | Message        | Description                                                       |
| ---------- | -------------- | ----------------------------------------------------------------- |
| -32602     | Invalid params | Block parameter is malformed or transaction index is out of range |
| -32603     | Internal error | Node failed to look up the block or transaction                   |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const tx = await provider.send('eth_getTransactionByBlockNumberAndIndex', ['latest', '0x0']);
console.log('First transaction in latest block:', tx.hash);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const tx = await client.getTransaction({
    blockTag: 'latest',
    index: 0,
});
console.log('First transaction in latest block:', tx.hash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
