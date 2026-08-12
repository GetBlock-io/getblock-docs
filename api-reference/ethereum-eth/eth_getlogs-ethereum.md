---
description: >-
  Example code for the eth_getLogs JSON RPC method. Сomplete guide on how to use
  eth_getLogs JSON RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Ethereum

This method returns all logs matching the given criteria across a block range — the most direct way to query historical events without creating a persistent filter. Ideal for one-off historical queries, indexer backfills, and any workflow where the filter criteria change per query.

## Parameters

| Parameter | Type   | Required | Description                                                   |
| --------- | ------ | -------- | ------------------------------------------------------------- |
| `filter`  | object | Yes      | Filter criteria — same shape as `eth_newFilter` filter object |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x171f000",
            "toBlock": "0x1720340",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
            ]
        }
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

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getLogs',
    params: [
        {
            "fromBlock": "0x171f000",
            "toBlock": "0x1720340",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
            ]
        }
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getLogs',
        'params': [
        {
            "fromBlock": "0x171f000",
            "toBlock": "0x1720340",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
            ]
        }
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getLogs",
            "params": [
        {
            "fromBlock": "0x171f000",
            "toBlock": "0x1720340",
            "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
            ]
        }
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
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                            |
| --------- | --------------- | ---------------------------------------------------------------------- |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                      |
| `id`      | string          | Request identifier matching the request                                |
| `result`  | array of object | Matching log entries in ascending order by (`blockNumber`, `logIndex`) |

## Use Cases

* **Indexer Backfill**: Iterate through historical block ranges to backfill contract events into an indexer database
* **Wallet Activity History**: Query all Transfer events for a specific address (as sender or receiver) across a block range
* **Analytics Queries**: Aggregate Swap events, deposits, or governance actions over time windows
* **Airdrop Snapshotting**: Retrieve all Transfer events for a token up to a snapshot block to compute historical holders

## Error Handling

| Error Code | Message                                            | Description                                               |
| ---------- | -------------------------------------------------- | --------------------------------------------------------- |
| -32602     | Invalid params                                     | Filter is malformed or block range is invalid             |
| 403        | Request block range exceeds the limit(1500 blocks) | Block range too wide — narrow the range or use pagination |
| -32603     | Internal error                                     | Node failed to retrieve matching logs                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const logs = await provider.getLogs({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    topics: ['0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'],
    fromBlock: 24248000,
    toBlock: 'latest',
});
console.log('Logs found:', logs.length);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, parseAbiItem } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/'),
});

const logs = await client.getLogs({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    event: parseAbiItem('event Transfer(address indexed from, address indexed to, uint256 value)'),
    fromBlock: 24248000n,
    toBlock: 'latest',
});
console.log('Logs found:', logs.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
