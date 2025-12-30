---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Complete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceBlockByHash - Base

The `debug_traceBlockByHash` method returns traces for all transactions in a block, identified by block hash. This is useful for analyzing all activity within a specific block.

## Parameters

| Parameter    | Type   | Required | Description               |
| ------------ | ------ | -------- | ------------------------- |
| blockHash    | string | Yes      | 32-byte hash of the block |
| traceOptions | object | No       | Tracing options object    |

### Trace Options

| Field        | Type   | Description                                               |
| ------------ | ------ | --------------------------------------------------------- |
| tracer       | string | Tracer type: "callTracer", "prestateTracer", or custom JS |
| tracerConfig | object | Configuration for the tracer                              |
| timeout      | string | Timeout for the trace operation                           |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const blockHash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceBlockByHash',
    params: [blockHash, { tracer: 'callTracer' }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const traces = response.data.result;
console.log('Total Transactions Traced:', traces.length);
traces.forEach((trace, index) => {
    console.log(`Tx ${index}: ${trace.result.type} - ${trace.result.from} -> ${trace.result.to}`);
});
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

block_hash = '0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceBlockByHash',
        'params': [block_hash, {'tracer': 'callTracer'}],
        'id': 'getblock.io'
    }
)

result = response.json()
traces = result['result']
print(f'Total Transactions Traced: {len(traces)}')
for i, trace in enumerate(traces):
    print(f'Tx {i}: {trace["result"]["type"]}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let block_hash = "0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "debug_traceBlockByHash",
            "params": [block_hash, {"tracer": "callTracer"}],
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
{% endtab %}
{% endtabs %}

## Response

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

* Block Analysis: Analyze all transactions in a block
* MEV Detection: Identify sandwich attacks and arbitrage
* Gas Profiling: Profile gas usage across block transactions
* Security Auditing: Trace internal calls for vulnerabilities
* Chain Forensics: Investigate block-level activity

## Error Handling

| Error Code | Message         | Description                   |
| ---------- | --------------- | ----------------------------- |
| -32602     | Invalid params  | Invalid block hash or options |
| -32603     | Internal error  | Trace execution failed        |
| -32000     | Block not found | Block does not exist          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function traceBlockByHash(blockHash) {
    const traces = await provider.send('debug_traceBlockByHash', [
        blockHash,
        { tracer: 'callTracer' }
    ]);
    
    console.log('Total Transactions:', traces.length);
    
    traces.forEach((trace, index) => {
        const result = trace.result;
        console.log(`\nTransaction ${index}:`);
        console.log('  Hash:', trace.txHash);
        console.log('  Type:', result.type);
        console.log('  From:', result.from);
        console.log('  To:', result.to);
        console.log('  Gas Used:', parseInt(result.gasUsed, 16));
        
        if (result.error) {
            console.log('  Error:', result.error);
        }
    });
    
    return traces;
}

traceBlockByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
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

async function traceBlockByHash(blockHash) {
    const traces = await client.request({
        method: 'debug_traceBlockByHash',
        params: [blockHash, { tracer: 'callTracer' }]
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

traceBlockByHash('0x849a3ac8f0d81df1a645701cdb9f90e58500d2eabb80ff3b7f4e8c13f025eff2');
```
{% endtab %}
{% endtabs %}
