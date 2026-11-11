---
description: >-
  Example code for the eth_getBlockReceipts JSON RPC method. Сomplete guide on
  how to use eth_getBlockReceipts JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Ethereum

This method returns all transaction receipts for a specific block in a single call. Introduced as a first-class RPC method more recently than per-transaction receipts, it replaces N separate `eth_getTransactionReceipt` calls with one batch — dramatically reducing round trips when indexing a block's activity.

## Parameters

| Parameter        | Type   | Required | Description                                                                              |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------------------- |
| `blockParameter` | string | Yes      | Block number in hex, block hash, or `latest`, `earliest`, `pending`, `finalized`, `safe` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": [
        "latest"
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
    method: 'eth_getBlockReceipts',
    params: [
        "latest"
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
        'method': 'eth_getBlockReceipts',
        'params': [
        "latest"
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
            "method": "eth_getBlockReceipts",
            "params": [
        "latest"
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
    "result": [
        {
            "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "blockHash": "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
            "blockNumber": "0x1720340",
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "cumulativeGasUsed": "0x33450",
            "gasUsed": "0x5208",
            "contractAddress": null,
            "logs": [
                {
                    "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045",
                        "0x000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48"
                    ],
                    "data": "0x000000000000000000000000000000000000000000000000016345785d8a0000",
                    "blockNumber": "0x1720340",
                    "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
                    "transactionIndex": "0x0",
                    "blockHash": "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
                    "logIndex": "0x0",
                    "removed": false
                }
            ],
            "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "status": "0x1",
            "type": "0x2",
            "effectiveGasPrice": "0x9502f900",
            "transactionIndex": "0x0",
            "blobGasUsed": "0x0",
            "blobGasPrice": "0x1"
        }
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                                                 |
| --------- | --------------- | ------------------------------------------------------------------------------------------- |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                                           |
| `id`      | string          | Request identifier matching the request                                                     |
| `result`  | array of object | Array of transaction receipts, one per transaction in the block, in transaction-index order |

## Use Cases

* **Log-Ingestion Pipelines**: Fetch all logs from a block in one call to feed an indexer or event-processing pipeline
* **Reduced-Round-Trip Indexing**: Replace N per-transaction receipt calls with 1 batch call — critical for indexer throughput
* **Block-Level Analytics**: Aggregate gas usage, fee revenue, or log counts across a block in a single query
* **Failed Transaction Detection**: Scan `status: "0x0"` receipts to find failed transactions in a block for MEV or anomaly analysis

## Error Handling

| Error Code | Message        | Description                                 |
| ---------- | -------------- | ------------------------------------------- |
| -32602     | Invalid params | Block parameter is malformed                |
| -32603     | Internal error | Node failed to compile the block's receipts |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const receipts = await provider.send('eth_getBlockReceipts', ['latest']);
console.log('Receipts in block:', receipts.length);

// Aggregate gas used across the block
const totalGas = receipts.reduce((sum, r) => sum + BigInt(r.gasUsed), 0n);
console.log('Total gas used:', totalGas.toString());
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

const receipts = await client.getBlockReceipts({ blockTag: 'latest' });
console.log('Receipts in block:', receipts.length);

const totalGas = receipts.reduce((sum, r) => sum + r.gasUsed, 0n);
console.log('Total gas used:', totalGas);
```
{% endcode %}
{% endtab %}
{% endtabs %}
