---
description: >-
  Example code for the eth_getFilterChanges JSON RPC method. Сomplete guide on
  how to use eth_getFilterChanges JSON RPC in GetBlock Web3 documentation.
---

# eth\_getFilterChanges - Ethereum

This method returns entries added to a filter since the last time it was polled. For log filters, returns matching log objects; for block filters, returns block hashes; for pending transaction filters, returns transaction hashes. Repeatedly polling this method drives a filter-based subscription loop.

## Parameters

| Parameter  | Type   | Required | Description                                                                                                       |
| ---------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------- |
| `filterId` | string | Yes      | Hex-encoded filter ID from a previous `eth_newFilter`, `eth_newBlockFilter`, or `eth_newPendingTransactionFilter` |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getFilterChanges",
    "params": [
        "0x16"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getFilterChanges',
    params: [
        "0x16"
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
        'method': 'eth_getFilterChanges',
        'params': [
        "0x16"
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
            "method": "eth_getFilterChanges",
            "params": [
        "0x16"
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

| Parameter | Type   | Description                                                                                                                                       |
| --------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                                                                                 |
| `id`      | string | Request identifier matching the request                                                                                                           |
| `result`  | array  | New entries since the last poll — log objects for log filters, block hashes for block filters, transaction hashes for pending transaction filters |

## Use Cases

* **Polling-Based Event Subscriptions**: Poll every few seconds to receive new matching logs since the last poll
* **Live Block Notifications**: Poll a block filter to receive new block hashes without WSS
* **Backfill After Reconnect**: After a reconnect or downtime, poll to receive all events buffered since the last successful poll

## Error Handling

| Error Code | Message        | Description                                      |
| ---------- | -------------- | ------------------------------------------------ |
| -32602     | Invalid params | Filter ID is malformed or the filter has expired |
| -32603     | Internal error | Node failed to read filter state                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

setInterval(async () => {
    const changes = await provider.send('eth_getFilterChanges', ['0x16']);
    changes.forEach(log => console.log('New log:', log.transactionHash));
}, 5000);
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

setInterval(async () => {
    const changes = await client.getFilterChanges({ filter });
    changes.forEach(log => console.log('New log:', log.transactionHash));
}, 5000);
```
{% endcode %}
{% endtab %}
{% endtabs %}
