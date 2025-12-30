---
description: >-
  Example code for the debug_traceTransaction  JSON-RPC method. Complete guide
  on how to use debug_traceTransaction  JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - Base

The `debug_traceTransaction` method returns a detailed trace of a transaction's execution. This method is essential for debugging smart contract interactions, understanding gas consumption, and analyzing transaction behavior.

## Parameters

| Parameter       | Type   | Required | Description              |
| --------------- | ------ | -------- | ------------------------ |
| transactionHash | string | Yes      | 32-byte transaction hash |
| traceOptions    | object | No       | Tracing options object   |

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
    "method": "debug_traceTransaction",
    "params": [
        "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
const axios = require('axios');

const txHash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceTransaction',
    params: [txHash, { tracer: 'callTracer' }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

const trace = response.data.result;
console.log('Type:', trace.type);
console.log('From:', trace.from);
console.log('To:', trace.to);
console.log('Gas Used:', trace.gasUsed);
```
{% endtab %}

{% tab title="Requests (Python)" %}
```python
import requests

tx_hash = '0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceTransaction',
        'params': [tx_hash, {'tracer': 'callTracer'}],
        'id': 'getblock.io'
    }
)

result = response.json()
trace = result['result']
print(f'Type: {trace["type"]}')
print(f'From: {trace["from"]}')
print(f'To: {trace["to"]}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let tx_hash = "0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "debug_traceTransaction",
            "params": [tx_hash, {"tracer": "callTracer"}],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Trace: {}", serde_json::to_string_pretty(&response["result"])?);
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
    "result": {
        "type": "CALL",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f5be21",
        "to": "0x4200000000000000000000000000000000000006",
        "value": "0x0",
        "gas": "0x5208",
        "gasUsed": "0x5208",
        "input": "0xa9059cbb...",
        "output": "0x0000000000000000000000000000000000000000000000000000000000000001",
        "calls": [
            {
                "type": "DELEGATECALL",
                "from": "0x...",
                "to": "0x...",
                "gas": "0x...",
                "gasUsed": "0x...",
                "input": "0x...",
                "output": "0x..."
            }
        ]
    }
}
```

## Response Parameters

| Parameter    | Type   | Description                                              |
| ------------ | ------ | -------------------------------------------------------- |
| type         | string | Call type (CALL, DELEGATECALL, STATICCALL, CREATE, etc.) |
| from         | string | Sender address                                           |
| to           | string | Recipient address                                        |
| value        | string | Value transferred in wei (hex)                           |
| gas          | string | Gas provided (hex)                                       |
| gasUsed      | string | Gas used (hex)                                           |
| input        | string | Call input data                                          |
| output       | string | Call output data                                         |
| calls        | array  | Nested internal calls                                    |
| error        | string | Error message if call failed                             |
| revertReason | string | Revert reason if available                               |

## Use Cases

* Transaction Debugging: Understanding Why Transactions Fail.
* Gas Optimization: Identify gas-heavy operations.
* Security Analysis: Trace internal calls for vulnerabilities.
* MEV Research: Analyze complex transaction patterns.
* Smart Contract Development: Debug contract logic.

## Error Handling

| Error Code | Message               | Description                         |
| ---------- | --------------------- | ----------------------------------- |
| -32602     | Invalid params        | Invalid transaction hash or options |
| -32603     | Internal error        | Trace execution failed              |
| -32000     | Transaction not found | Transaction does not exist          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js (v6)" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function traceTransaction(txHash) {
    // Call tracer for internal calls
    const callTrace = await provider.send('debug_traceTransaction', [
        txHash,
        { tracer: 'callTracer' }
    ]);
    
    console.log('Call Trace:', JSON.stringify(callTrace, null, 2));
    
    // Prestate tracer for state changes
    const prestateTrace = await provider.send('debug_traceTransaction', [
        txHash,
        { tracer: 'prestateTracer' }
    ]);
    
    console.log('Prestate:', JSON.stringify(prestateTrace, null, 2));
    
    return { callTrace, prestateTrace };
}

traceTransaction('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');

// Recursive function to analyze call tree
function analyzeCallTree(call, depth = 0) {
    const indent = '  '.repeat(depth);
    console.log(`${indent}${call.type}: ${call.from} -> ${call.to}`);
    console.log(`${indent}  Gas: ${parseInt(call.gasUsed, 16)}`);
    
    if (call.error) {
        console.log(`${indent}  Error: ${call.error}`);
    }
    
    if (call.calls) {
        call.calls.forEach(subcall => analyzeCallTree(subcall, depth + 1));
    }
}
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

async function traceTransaction(txHash) {
    const trace = await client.request({
        method: 'debug_traceTransaction',
        params: [txHash, { tracer: 'callTracer' }]
    });
    
    console.log('Type:', trace.type);
    console.log('From:', trace.from);
    console.log('To:', trace.to);
    console.log('Gas Used:', parseInt(trace.gasUsed, 16));
    
    if (trace.calls) {
        console.log('Internal Calls:', trace.calls.length);
    }
    
    return trace;
}

traceTransaction('0x633982a26e0cfba940613c52b31c664fe977e05171e35f62da2426596007e249');
```
{% endtab %}
{% endtabs %}
