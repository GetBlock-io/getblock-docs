---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - Monad

This method returns the trace of a transaction execution, including all internal calls and state changes.

## Parameters

| Parameter                 | Type   | Required | Description                                       |
| ------------------------- | ------ | -------- | ------------------------------------------------- |
| transactionHash           | string | Yes      | The 32-byte transaction hash.                     |
| traceOptions              | object | **Yes**  | Trace configuration options (required on Monad).  |
| traceOptions.tracer       | string | No       | Type of tracer: "callTracer" or "prestateTracer". |
| traceOptions.tracerConfig | object | No       | Configuration for the tracer.                     |
| traceOptions.timeout      | string | No       | Timeout for the trace operation.                  |

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
        "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "debug_traceTransaction",
            "params": [
                "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
                {"tracer": "callTracer"}
            ],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response (callTracer)

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "type": "CALL",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f0beb",
        "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "value": "0x0",
        "gas": "0x5208",
        "gasUsed": "0x5028",
        "input": "0xa9059cbb000000000000000000000000...",
        "output": "0x0000000000000000000000000000000000000000000000000000000000000001",
        "calls": [
            {
                "type": "CALL",
                "from": "0xdac17f958d2ee523a2206206994597c13d831ec7",
                "to": "0x1234567890123456789012345678901234567890",
                "value": "0x0",
                "gas": "0x4000",
                "gasUsed": "0x1000",
                "input": "0x...",
                "output": "0x..."
            }
        ]
    }
}
```

## Response Parameters (callTracer)

| Field        | Type   | Description                                                 |
| ------------ | ------ | ----------------------------------------------------------- |
| type         | string | Call type: CALL, STATICCALL, DELEGATECALL, CREATE, CREATE2. |
| from         | string | Sender address.                                             |
| to           | string | Recipient address.                                          |
| value        | string | Value transferred in wei.                                   |
| gas          | string | Gas provided for the call.                                  |
| gasUsed      | string | Gas consumed by the call.                                   |
| input        | string | Input data to the call.                                     |
| output       | string | Return data from the call.                                  |
| error        | string | Error message if call reverted.                             |
| revertReason | string | Decoded revert reason if available.                         |
| calls        | array  | Nested internal calls.                                      |

## Tracer Types

### callTracer

Returns a hierarchical tree of all calls made during execution.

```json
{"tracer": "callTracer"}
```

### prestateTracer

Returns the state accessed/modified during execution.

```json
{"tracer": "prestateTracer"}
```

## Use Case

The `debug_traceTransaction` method is essential for:

* Debugging failed transactions
* Analyzing internal calls
* Security auditing
* Gas optimization analysis
* Block explorer trace views
* Understanding complex DeFi interactions

## Error Handling

| Status Code | Error Message      | Cause                                      |
| ----------- | ------------------ | ------------------------------------------ |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.           |
| -32602      | Invalid params     | Missing trace options (required on Monad). |
| -32000      | Resource not found | Transaction not found.                     |
| -32000      | Timeout            | Trace took too long.                       |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Must include trace options object (required on Monad)
const trace = await provider.send('debug_traceTransaction', [
    txHash,
    { tracer: 'callTracer' }
]);

console.log('Call type:', trace.type);
console.log('From:', trace.from);
console.log('To:', trace.to);
console.log('Internal calls:', trace.calls?.length || 0);
```
{% endtab %}
{% endtabs %}
