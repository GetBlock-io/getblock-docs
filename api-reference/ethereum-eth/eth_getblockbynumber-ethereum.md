---
description: >-
  Example code for the eth_getBlockByNumber JSON RPC method. Сomplete guide on
  how to use eth_getBlockByNumber JSON RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - Ethereum

This method returns block data given a block number or tag. The block reference can be a hex-encoded number or one of the tags `latest`, `earliest`, `pending`, `finalized` (post-Merge finalized block), or `safe` (justified block). This is the most common block-lookup method — used by every block explorer, indexer, and chain-observation tool.

## Parameters

| Parameter                | Type    | Required | Description                                                                              |
| ------------------------ | ------- | -------- | ---------------------------------------------------------------------------------------- |
| `blockParameter`         | string  | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe`             |
| `fullTransactionObjects` | boolean | Yes      | If `true`, returns full transaction objects; if `false`, returns just transaction hashes |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "latest",
        false
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
    method: 'eth_getBlockByNumber',
    params: [
        "latest",
        false
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
        'method': 'eth_getBlockByNumber',
        'params': [
        "latest",
        false
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
            "method": "eth_getBlockByNumber",
            "params": [
        "latest",
        false
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
        "number": "0x1720340",
        "hash": "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        "parentHash": "0x8db7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b",
        "nonce": "0x0000000000000000",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "transactionsRoot": "0x9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "stateRoot": "0x5eae9c5cbe0e8d3e2f5cd8b0a4e3b1f2c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4",
        "receiptsRoot": "0x3a2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b",
        "miner": "0x1f9090aae28b8a3dceadf281b0f12828e676c326",
        "difficulty": "0x0",
        "totalDifficulty": "0xc70d815d562d3cfa955",
        "extraData": "0x546974616e2028746974616e6275696c6465722e78797a29",
        "size": "0x28a4b",
        "gasLimit": "0x39dc7fb",
        "gasUsed": "0x1c7db2c",
        "timestamp": "0x67abcdef",
        "baseFeePerGas": "0x5f5e100",
        "blobGasUsed": "0x180000",
        "excessBlobGas": "0x2a0000",
        "withdrawalsRoot": "0x1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3",
        "parentBeaconBlockRoot": "0x2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4",
        "requestsHash": "0x3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5",
        "mixHash": "0x9ac2e6f3a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6",
        "uncles": [],
        "transactions": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            "0x1cd7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b"
        ],
        "withdrawals": [
            {
                "index": "0x1a2b3c",
                "validatorIndex": "0x1234",
                "address": "0x1234567890abcdef1234567890abcdef12345678",
                "amount": "0x1c9c380"
            }
        ]
    }
}
```

## Response Parameters

| Parameter                      | Type            | Description                                                                      |
| ------------------------------ | --------------- | -------------------------------------------------------------------------------- |
| `jsonrpc`                      | string          | JSON-RPC protocol version ("2.0")                                                |
| `id`                           | string          | Request identifier matching the request                                          |
| `result.number`                | string          | Hex-encoded block number                                                         |
| `result.hash`                  | string          | 32-byte block hash                                                               |
| `result.parentHash`            | string          | Hash of the parent block                                                         |
| `result.nonce`                 | string          | 8-byte block nonce (deprecated post-Merge, always `0x0000000000000000`)          |
| `result.sha3Uncles`            | string          | keccak256 of the uncles list (empty post-Merge)                                  |
| `result.logsBloom`             | string          | 256-byte bloom filter over the block's logs                                      |
| `result.transactionsRoot`      | string          | Root of the transactions trie                                                    |
| `result.stateRoot`             | string          | Root of the state trie after processing the block                                |
| `result.receiptsRoot`          | string          | Root of the receipts trie                                                        |
| `result.miner`                 | string          | Address that received the block-level fee (fee recipient / proposer)             |
| `result.difficulty`            | string          | Block difficulty (`0x0` post-Merge)                                              |
| `result.totalDifficulty`       | string          | Cumulative chain difficulty at this block                                        |
| `result.extraData`             | string          | Arbitrary extra data (up to 32 bytes)                                            |
| `result.size`                  | string          | Size of the block in bytes (hex)                                                 |
| `result.gasLimit`              | string          | Maximum gas allowed in the block (hex)                                           |
| `result.gasUsed`               | string          | Actual gas consumed by transactions in the block                                 |
| `result.timestamp`             | string          | Unix timestamp of block production (hex)                                         |
| `result.baseFeePerGas`         | string          | EIP-1559 base fee per gas (wei, hex)                                             |
| `result.blobGasUsed`           | string          | Total blob gas consumed by blob transactions in the block (post-Cancun)          |
| `result.excessBlobGas`         | string          | Excess blob gas carried into next block (post-Cancun)                            |
| `result.withdrawalsRoot`       | string          | Root of the block's withdrawals list (post-Shapella)                             |
| `result.parentBeaconBlockRoot` | string          | Beacon block root of the parent (post-Cancun / EIP-4788)                         |
| `result.requestsHash`          | string          | Hash of the block's execution-layer requests list (post-Pectra / EIP-7685)       |
| `result.mixHash`               | string          | Prev-randao value (post-Merge, formerly PoW mix digest)                          |
| `result.uncles`                | array of string | Uncle block hashes (empty post-Merge)                                            |
| `result.transactions`          | array           | Transaction hashes, or full transaction objects if `fullTransactionObjects=true` |
| `result.withdrawals`           | array of object | Consensus-layer withdrawals credited in the block (post-Shapella)                |

## Use Cases

* **Latest Block Polling**: Poll `latest` to detect chain progress and drive per-block indexing workflows
* **Finality-Aware Reads**: Use `finalized` to read from the beacon-chain-finalized tip, immune to reorgs
* **Historical Backfill**: Iterate through historical block numbers to backfill an indexer or analytics database
* **Transaction Enumeration**: Pass `fullTransactionObjects=true` to fetch every transaction in a block in a single call

## Error Handling

| Error Code | Message        | Description                                                             |
| ---------- | -------------- | ----------------------------------------------------------------------- |
| -32602     | Invalid params | Block parameter is malformed or a numeric value is beyond the chain tip |
| -32603     | Internal error | Node failed to retrieve the block                                       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// Latest block
const latest = await provider.getBlock('latest');
console.log('Latest:', latest.number);

// Finality-aware read
const finalized = await provider.getBlock('finalized');
console.log('Finalized:', finalized.number);

// With full transactions
const blockWithTxs = await provider.getBlock('latest', true);
console.log('Transactions:', blockWithTxs.transactions.length);
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

const latest = await client.getBlock({ blockTag: 'latest' });
console.log('Latest:', latest.number);

const finalized = await client.getBlock({ blockTag: 'finalized' });
console.log('Finalized:', finalized.number);

const blockWithTxs = await client.getBlock({ blockTag: 'latest', includeTransactions: true });
console.log('Transactions:', blockWithTxs.transactions.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
