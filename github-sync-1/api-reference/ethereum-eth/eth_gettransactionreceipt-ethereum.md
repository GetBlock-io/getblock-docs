---
description: >-
  Example code for the eth_getTransactionReceipt JSON RPC method. Сomplete guide
  on how to use eth_getTransactionReceipt JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionReceipt- Ethereum

This method returns the receipt for a transaction — status (success/reverted), gas used, event logs emitted, and post-EIP-4844 blob-related fields. Receipts are only available after the transaction is mined; polling this method is the standard way to detect confirmation and read emitted events after submission.

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| `txHash`  | string | Yes      | 32-byte transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionReceipt",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
    method: 'eth_getTransactionReceipt',
    params: [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
        'method': 'eth_getTransactionReceipt',
        'params': [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
            "method": "eth_getTransactionReceipt",
            "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
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
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "status": "0x1",
        "type": "0x2",
        "effectiveGasPrice": "0x9502f900",
        "transactionIndex": "0x0",
        "blobGasUsed": "0x0",
        "blobGasPrice": "0x1"
    }
}
```

## Response Parameters

| Parameter                  | Type            | Description                                                                                                  |
| -------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------ |
| `jsonrpc`                  | string          | JSON-RPC protocol version ("2.0")                                                                            |
| `id`                       | string          | Request identifier matching the request                                                                      |
| `result.transactionHash`   | string          | The transaction hash                                                                                         |
| `result.blockHash`         | string          | Hash of the containing block                                                                                 |
| `result.blockNumber`       | string          | Block number of the containing block                                                                         |
| `result.from`              | string          | Sender address                                                                                               |
| `result.to`                | string          | Recipient address (`null` for contract creation)                                                             |
| `result.cumulativeGasUsed` | string          | Total gas consumed in the block up to and including this transaction                                         |
| `result.gasUsed`           | string          | Actual gas consumed by this transaction                                                                      |
| `result.contractAddress`   | string          | Address of newly deployed contract (`null` for regular transactions)                                         |
| `result.logs`              | array of object | Event logs emitted during execution                                                                          |
| `result.logsBloom`         | string          | Bloom filter over this transaction's logs                                                                    |
| `result.status`            | string          | `0x1` = success, `0x0` = reverted                                                                            |
| `result.type`              | string          | Transaction type (`0x0` legacy, `0x1` EIP-2930, `0x2` EIP-1559, `0x3` EIP-4844 blob, `0x4` EIP-7702 SetCode) |
| `result.effectiveGasPrice` | string          | Effective gas price paid                                                                                     |
| `result.transactionIndex`  | string          | Position of this transaction within its block                                                                |
| `result.blobGasUsed`       | string          | Blob gas consumed (EIP-4844 blob transactions only)                                                          |
| `result.blobGasPrice`      | string          | Blob gas price paid (EIP-4844 blob transactions only)                                                        |

## Use Cases

* **Post-Submit Confirmation**: Poll after `eth_sendRawTransaction` to detect when a transaction is mined and confirm success/revert
* **Event Log Extraction**: Read `logs` to capture emitted events (ERC-20 Transfer, Uniswap Swap, etc.) for indexers and dApps
* **Actual-Cost Calculation**: Compute the actual transaction cost as `gasUsed × effectiveGasPrice`
* **Contract Deployment Address Discovery**: Read `contractAddress` after a factory deployment to get the new contract's address

## Error Handling

| Error Code | Message        | Description                                                                            |
| ---------- | -------------- | -------------------------------------------------------------------------------------- |
| -32602     | Invalid params | Transaction hash is malformed                                                          |
| -32603     | Internal error | Node failed to look up the receipt (usually indicates the transaction isn't mined yet) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const receipt = await provider.getTransactionReceipt('0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b');

if (!receipt) {
    console.log('Not mined yet');
} else if (receipt.status === 1) {
    console.log('Success');
    console.log('Gas used:', receipt.gasUsed.toString());
    console.log('Effective gas price:', ethers.formatUnits(receipt.gasPrice, 'gwei'), 'gwei');
    console.log('Logs emitted:', receipt.logs.length);
} else {
    console.log('Reverted');
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const receipt = await client.getTransactionReceipt({ hash: '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b' });
console.log('Status:', receipt.status);
console.log('Gas used:', receipt.gasUsed);
console.log('Effective gas price:', formatGwei(receipt.effectiveGasPrice), 'gwei');
console.log('Logs emitted:', receipt.logs.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
