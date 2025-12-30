---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide on how to use
  eth_syncing JSON-RPC in GetBlock Web3 documentation.
---

# eth\_syncing - Base

The `eth_syncing` method returns an object with data about the sync status or `false` if the node is fully synced. This is used to check if a node is still synchronizing with the network.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_syncing',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const syncStatus = response.data.result;
if (syncStatus === false) {
    console.log('Node is fully synced');
} else {
    console.log('Syncing...');
    console.log('Current Block:', parseInt(syncStatus.currentBlock, 16));
    console.log('Highest Block:', parseInt(syncStatus.highestBlock, 16));
}
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_syncing',
        'params': [],
        'id': 'getblock.io'
    }
)

result = response.json()
sync_status = result['result']
if sync_status is False:
    print('Node is fully synced')
else:
    print('Syncing...')
    print(f'Current Block: {int(sync_status["currentBlock"], 16)}')
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
            "method": "eth_syncing",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    match &response["result"] {
        Value::Bool(false) => println!("Node is fully synced"),
        Value::Object(obj) => println!("Syncing: {:?}", obj),
        _ => println!("Unknown status"),
    }
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response (Synced)

{% tabs %}
{% tab title="Synced" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```
{% endtab %}

{% tab title="Syncing" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "startingBlock": "0x0",
        "currentBlock": "0x12d6800",
        "highestBlock": "0x12d687f"
    }
}
```
{% endtab %}
{% endtabs %}

## Response Parameters

| Parameter | Type           | Description                                     |
| --------- | -------------- | ----------------------------------------------- |
| result    | boolean/object | `false` if synced, otherwise sync status object |

### Sync Status Object

| Field         | Type   | Description                       |
| ------------- | ------ | --------------------------------- |
| startingBlock | string | Block where sync started (hex)    |
| currentBlock  | string | Current sync progress block (hex) |
| highestBlock  | string | Estimated highest block (hex)     |

## Use Cases

* Health Checks: Verify node is fully synced before operations.
* Sync Monitoring: Track synchronization progress.
* Load Balancing: Route requests only to synced nodes.
* Application Startup: Wait for node sync before processing.
* Alerting: Detect nodes falling behind.

## Error Handling

| Error Code | Message        | Description           |
| ---------- | -------------- | --------------------- |
| -32603     | Internal error | Node internal failure |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function checkSyncStatus() {
    const syncStatus = await provider.send('eth_syncing', []);
    
    if (syncStatus === false) {
        console.log('Node is fully synced');
        
        // Verify with block number
        const blockNumber = await provider.getBlockNumber();
        console.log('Current Block:', blockNumber);
    } else {
        console.log('Node is syncing...');
        console.log('Starting Block:', parseInt(syncStatus.startingBlock, 16));
        console.log('Current Block:', parseInt(syncStatus.currentBlock, 16));
        console.log('Highest Block:', parseInt(syncStatus.highestBlock, 16));
        
        const progress = (parseInt(syncStatus.currentBlock, 16) / parseInt(syncStatus.highestBlock, 16)) * 100;
        console.log('Progress:', progress.toFixed(2) + '%');
    }
    
    return syncStatus;
}

checkSyncStatus();
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function checkSyncStatus() {
    const syncStatus = await client.request({
        method: 'eth_syncing',
        params: []
    });
    
    if (syncStatus === false) {
        console.log('Node is fully synced');
        
        // Get current block as verification
        const blockNumber = await client.getBlockNumber();
        console.log('Current Block:', blockNumber);
    } else {
        console.log('Node is syncing...');
        console.log('Current:', parseInt(syncStatus.currentBlock, 16));
        console.log('Highest:', parseInt(syncStatus.highestBlock, 16));
    }
    
    return syncStatus;
}

checkSyncStatus();
```
{% endtab %}
{% endtabs %}
