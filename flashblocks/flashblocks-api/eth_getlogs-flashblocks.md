---
description: >-
  Example code for the eth_getLogs JSON-RPC method. Complete guide on how to use
  eth_getLogs JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getLogs - Flashblocks

This returns query logs from pre-confirmed transactions, updated every \~200ms.&#x20;

## Parameters

| Parameter    | Type   | Required | Description           |
| ------------ | ------ | -------- | --------------------- |
| filterObject | object | Yes      | Filter options object |

### Filter Object

| Field     | Type         | Required | Description                                                   |
| --------- | ------------ | -------- | ------------------------------------------------------------- |
| fromBlock | string       | No       | Start block number (hex) or "latest", "earliest" or "pending" |
| toBlock   | string       | No       | End block number (hex) or "latest", "earliest"                |
| address   | string/array | No       | Contract address or array of addresses                        |
| topics    | array        | No       | Array of topic filters (up to 4 topics)                       |
| blockHash | string       | No       | Specific block hash (alternative to fromBlock/toBlock)        |

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
      "fromBlock": "pending",
      "toBlock": "pending",
      "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]
    }],
    "id": 1
  }'
```
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
const axios = require('axios');

// Query ERC-20 Transfer events
const transferTopic = '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
      "fromBlock": "pending",
      "toBlock": "pending",
      "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913"
    }],
    "id": 1
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
{% endcode %}
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
    "jsonrpc": "2.0",
    "method": "eth_getLogs",
    "params": [{
      "fromBlock": "pending",
      "toBlock": "pending",
      "address": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    }],
    "id": "getblock.io"
  }
)

result = response.json()
logs = result['result']
print(f'Found {len(logs)} transfer events')
for log in logs:
    print(f'Block: {int(log["blockNumber"], 16)}')
```
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "result": [
        {
            "address": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000b2cc224c1c9fee385f8ad6a55b4d94e92359dc59",
                "0x00000000000000000000000051c72848c68a965f66fa7a88855f9f7784502a7f"
            ],
            "data": "0x00000000000000000000000000000000000000000000000000000000c2c9d499",
            "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "blockNumber": "0x2ddffa0",
            "blockTimestamp": "0x6a465c23",
            "transactionHash": "0xfe22be1e04c2b149559b7244f4c5b03693a071cfa5a834590dc16bed318ebeab",
            "transactionIndex": "0x1",
            "logIndex": "0x0",
            "removed": false
        },
        {
            "address": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x0000000000000000000000004e962bb3889bf030368f56810a9c96b83cb3e778",
                "0x00000000000000000000000051c72848c68a965f66fa7a88855f9f7784502a7f"
            ],
            "data": "0x00000000000000000000000000000000000000000000000000000001309ccb9c",
            "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "blockNumber": "0x2ddffa0",
            "blockTimestamp": "0x6a465c23",
            "transactionHash": "0x201d890aee2c4b934801032f2b72e888c6a332cda7ccd76cf9e549bd166bf91a",
            "transactionIndex": "0x3",
            "logIndex": "0x3",
            "removed": false
        },
        {
            "address": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d4dc229a997a73e86c13e3ce8019be02cd4827a6",
                "0x0000000000000000000000009e7243b98a8a9713cb9dc37fe9f18879f0e64dcb"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000000000000d7057c",
            "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "blockNumber": "0x2ddffa0",
            "blockTimestamp": "0x6a465c23",
            "transactionHash": "0x0f9c2aee37cff0d3cc5564c685923f6d19ec5c7c6a0d7234ee6fe06655950a2b",
            "transactionIndex": "0xb",
            "logIndex": "0x7",
            "removed": false
        },
        {
            "address": "0x833589fcd6edb6e08f4c7c32d4f71b54bda02913",
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x00000000000000000000000040378d78b14f46c35e2f69590b733e5275ac1d3b",
                "0x000000000000000000000000fb08f1c035c05d5c12e12657baee1d1d951c2c6c"
            ],
            "data": "0x0000000000000000000000000000000000000000000000000000000005f5e100",
            "blockHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "blockNumber": "0x2ddffa0",
            "blockTimestamp": "0x6a465c23",
            "transactionHash": "0xc93d2aca48444c29d5538c2a79fe8bc13047f4efa656e0cc6877db0de8ea553c",
            "transactionIndex": "0xd",
            "logIndex": "0xb",
            "removed": false
        },
    ],
    "id": "getblock.io"
}
```
{% endcode %}

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

## SDK Integration

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
        fromBlock: 'pending',
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
{% code overflow="wrap" %}
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
        fromBlock: 'pending',
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
{% endcode %}
{% endtab %}
{% endtabs %}
