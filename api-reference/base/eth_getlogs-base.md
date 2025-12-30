---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Base

The `eth_getLogs` method returns an array of all logs matching a given filter object. This is the primary method for querying event logs from smart contracts, essential for tracking token transfers, DEX swaps, and other on-chain events.

## Parameters

| Parameter    | Type   | Required | Description           |
| ------------ | ------ | -------- | --------------------- |
| filterObject | object | Yes      | Filter options object |

### Filter Object

| Field     | Type         | Required | Description                                            |
| --------- | ------------ | -------- | ------------------------------------------------------ |
| fromBlock | string       | No       | Start block number (hex) or "latest", "earliest"       |
| toBlock   | string       | No       | End block number (hex) or "latest", "earliest"         |
| address   | string/array | No       | Contract address or array of addresses                 |
| topics    | array        | No       | Array of topic filters (up to 4 topics)                |
| blockHash | string       | No       | Specific block hash (alternative to fromBlock/toBlock) |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
        "fromBlock": "0x12d6800",
        "toBlock": "0x12d6900",
        "address": "0x4200000000000000000000000000000000000006",
        "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": "getblock.io"
}'
```


{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

// Query ERC-20 Transfer events
const transferTopic = '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getLogs',
    params: [{
        fromBlock: '0x12d6800',
        toBlock: '0x12d6900',
        address: '0x4200000000000000000000000000000000000006', // WETH
        topics: [transferTopic]
    }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const logs = response.data.result;
console.log('Found', logs.length, 'transfer events');
logs.forEach(log => {
    console.log('Block:', parseInt(log.blockNumber, 16));
    console.log('Tx:', log.transactionHash);
});
```


{% endtab %}

{% tab title="Request" %}
```python
import requests

# ERC-20 Transfer event signature
transfer_topic = '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getLogs',
        'params': [{
            'fromBlock': '0x12d6800',
            'toBlock': '0x12d6900',
            'address': '0x4200000000000000000000000000000000000006',
            'topics': [transfer_topic]
        }],
        'id': 'getblock.io'
    }
)

result = response.json()
logs = result['result']
print(f'Found {len(logs)} transfer events')
for log in logs:
    print(f'Block: {int(log["blockNumber"], 16)}')
```


{% endtab %}

{% tab title="Rust" %}
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
            "params": [{
                "fromBlock": "0x12d6800",
                "toBlock": "0x12d6900",
                "address": "0x4200000000000000000000000000000000000006",
                "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
            }],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    let logs = response["result"].as_array().unwrap();
    println!("Found {} logs", logs.len());
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "address": "0x4200000000000000000000000000000000000006",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000sender_address_padded",
                "0x000000000000000000000000recipient_address_padded"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
            "blockNumber": "0x12d6850",
            "transactionHash": "0x...",
            "transactionIndex": "0x0",
            "blockHash": "0x...",
            "logIndex": "0x0",
            "removed": false
        }
    ]
}
```

## Response Parameters

| Parameter        | Type    | Description                                |
| ---------------- | ------- | ------------------------------------------ |
| address          | string  | Contract address that emitted the log      |
| topics           | array   | Indexed event parameters (up to 4)         |
| data             | string  | Non-indexed event data (hex)               |
| blockNumber      | string  | Block number in hex                        |
| transactionHash  | string  | Transaction hash                           |
| transactionIndex | string  | Transaction index in block (hex)           |
| blockHash        | string  | Block hash                                 |
| logIndex         | string  | Log index in block (hex)                   |
| removed          | boolean | True if log was removed due to chain reorg |

## Use Cases

* Token Transfer Tracking: Monitor ERC-20/ERC-721 transfers
* DEX Event Processing: Track swaps, liquidity events
* Contract Event Indexing: Build event-driven databases
* Real-time Notifications: Alert on specific events
* Historical Analysis: Analyze past contract activity

## Error Handling

| Error Code | Message                        | Description                           |
| ---------- | ------------------------------ | ------------------------------------- |
| -32602     | Invalid params                 | Invalid filter object                 |
| -32603     | Internal error                 | Query range too large or node failure |
| -32005     | Query returned more than limit | Too many logs in response             |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getLogs() {
    // Using filter
    const filter = {
        address: '0x4200000000000000000000000000000000000006', // WETH
        topics: [
            ethers.id('Transfer(address,address,uint256)')
        ],
        fromBlock: 'latest',
        toBlock: 'latest'
    };
    
    const logs = await provider.getLogs(filter);
    
    logs.forEach(log => {
        console.log('Block:', log.blockNumber);
        console.log('Tx:', log.transactionHash);
        console.log('Data:', log.data);
    });
    
    return logs;
}

// Using Contract interface for parsed events
async function getTransferEvents() {
    const abi = ['event Transfer(address indexed from, address indexed to, uint256 value)'];
    const contract = new ethers.Contract(
        '0x4200000000000000000000000000000000000006',
        abi,
        provider
    );
    
    const filter = contract.filters.Transfer();
    const events = await contract.queryFilter(filter, -100, 'latest');
    
    events.forEach(event => {
        console.log('From:', event.args.from);
        console.log('To:', event.args.to);
        console.log('Value:', ethers.formatEther(event.args.value));
    });
    
    return events;
}

getLogs();
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, parseAbiItem } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function getLogs() {
    const logs = await client.getLogs({
        address: '0x4200000000000000000000000000000000000006',
        event: parseAbiItem('event Transfer(address indexed from, address indexed to, uint256 value)'),
        fromBlock: BigInt(19800000),
        toBlock: 'latest'
    });
    
    logs.forEach(log => {
        console.log('From:', log.args.from);
        console.log('To:', log.args.to);
        console.log('Value:', log.args.value);
    });
    
    return logs;
}

// Watch for new logs
async function watchLogs() {
    const unwatch = client.watchEvent({
        address: '0x4200000000000000000000000000000000000006',
        event: parseAbiItem('event Transfer(address indexed from, address indexed to, uint256 value)'),
        onLogs: logs => {
            logs.forEach(log => {
                console.log('New Transfer:', log.args);
            });
        }
    });
    
    return unwatch;
}

getLogs();
```
{% endtab %}
{% endtabs %}
