---
description: >-
  Example code for the debug_traceBlockByNumber JSON-RPC method. Complete guide
  on how to use debug_traceBlockByNumber JSON-RPC in GetBlock Web3
  documentation.
---

# debug\_traceBlockByNumber - Base

The `debug_traceBlockByNumber` method returns traces for all transactions in a block, identified by block number. This is useful for analyzing all activity within a specific block when you know the block number.

## Parameters

| Parameter    | Type   | Required | Description                                             |
| ------------ | ------ | -------- | ------------------------------------------------------- |
| blockNumber  | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |
| traceOptions | object | No       | Tracing options object                                  |

### Trace Options

| Field        | Type   | Description                                               |
| ------------ | ------ | --------------------------------------------------------- |
| tracer       | string | Tracer type: "callTracer", "prestateTracer", or custom JS |
| tracerConfig | object | Configuration for the tracer                              |
| timeout      | string | Timeout for the trace operation                           |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByNumber",
    "params": ["0x12d687f", {"tracer": "callTracer"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios-example.js" %}
```javascript
const axios = require('axios');

const blockNumber = '0x12d687f';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceBlockByNumber',
    params: [blockNumber, { tracer: 'callTracer' }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const traces = response.data.result;
console.log('Total Transactions Traced:', traces.length);
traces.forEach((trace, index) => {
    console.log(`Tx ${index}: ${trace.result.type}`);
});
```
{% endcode %}
{% endtab %}

{% tab title="Requests (Python)" %}
{% code title="trace_block.py" %}
```python
import requests

block_number = '0x12d687f'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceBlockByNumber',
        'params': [block_number, {'tracer': 'callTracer'}],
        'id': 'getblock.io'
    }
)

result = response.json()
traces = result['result']
print(f'Total Transactions Traced: {len(traces)}')
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
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
            "method": "debug_traceBlockByNumber",
            "params": ["0x12d687f", {"tracer": "callTracer"}],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    let traces = response["result"].as_array().unwrap();
    println!("Total Traces: {}", traces.len());
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "txHash": "0x...",
            "result": {
                "type": "CALL",
                "from": "0x...",
                "to": "0x...",
                "value": "0x0",
                "gas": "0x...",
                "gasUsed": "0x...",
                "input": "0x...",
                "output": "0x...",
                "calls": []
            }
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | array  | Array of transaction traces             |

### Trace Object

| Field  | Type   | Description         |
| ------ | ------ | ------------------- |
| txHash | string | Transaction hash    |
| result | object | Trace result object |

## Use Cases

* Sequential Analysis: Analyze blocks in sequence by number
* Historical Research: Trace historical block activity
* Gas Analysis: Profile gas usage per block
* Indexing: Build transaction traces for indexing
* Monitoring: Track activity at specific block heights

## Error Handling

| Error Code | Message         | Description                     |
| ---------- | --------------- | ------------------------------- |
| -32602     | Invalid params  | Invalid block number or options |
| -32603     | Internal error  | Trace execution failed          |
| -32000     | Block not found | Block does not exist            |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
{% code title="ethers-trace.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function traceBlockByNumber(blockNumber) {
    // Convert number to hex if needed
    const blockHex = typeof blockNumber === 'number' 
        ? '0x' + blockNumber.toString(16) 
        : blockNumber;
    
    const traces = await provider.send('debug_traceBlockByNumber', [
        blockHex,
        { tracer: 'callTracer' }
    ]);
    
    console.log('Block:', blockNumber);
    console.log('Total Transactions:', traces.length);
    
    let totalGas = 0n;
    traces.forEach((trace, index) => {
        const gasUsed = BigInt(trace.result.gasUsed);
        totalGas += gasUsed;
        
        console.log(`\nTransaction ${index}:`);
        console.log('  Type:', trace.result.type);
        console.log('  Gas Used:', gasUsed.toString());
    });
    
    console.log('\nTotal Gas Used:', totalGas.toString());
    
    return traces;
}

// Trace latest block
async function traceLatestBlock() {
    const blockNumber = await provider.getBlockNumber();
    return traceBlockByNumber(blockNumber);
}

traceLatestBlock();
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-trace.js" %}
```javascript
import { createPublicClient, http, numberToHex } from 'viem';
import { base } from 'viem/chains';

const client = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

async function traceBlockByNumber(blockNumber) {
    const blockHex = typeof blockNumber === 'bigint' 
        ? numberToHex(blockNumber) 
        : blockNumber;
    
    const traces = await client.request({
        method: 'debug_traceBlockByNumber',
        params: [blockHex, { tracer: 'callTracer' }]
    });
    
    console.log('Total Transactions:', traces.length);
    
    traces.forEach((trace, index) => {
        console.log(`Tx ${index}:`, {
            hash: trace.txHash,
            type: trace.result.type,
            gasUsed: parseInt(trace.result.gasUsed, 16)
        });
    });
    
    return traces;
}

// Trace specific block
traceBlockByNumber('0x12d687f');

// Trace latest
async function traceLatest() {
    const blockNumber = await client.getBlockNumber();
    return traceBlockByNumber(blockNumber);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
