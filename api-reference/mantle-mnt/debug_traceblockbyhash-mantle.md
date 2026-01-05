---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Complete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceBlockByHash - Mantle

This method traces the execution of all transactions within a block specified by block hash on the Mantle network. This is a premium method available on Mantle Sepolia Testnet.

### Parameters

| Parameter    | Type   | Description                |
| ------------ | ------ | -------------------------- |
| blockHash    | string | 32-byte block hash         |
| traceOptions | object | (optional) Tracing options |

Trace Options:

| Field        | Type   | Description                                 |
| ------------ | ------ | ------------------------------------------- |
| tracer       | string | Tracer type: "callTracer", "prestateTracer" |
| timeout      | string | Timeout for the trace                       |
| tracerConfig | object | Configuration for the tracer                |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        {"tracer": "callTracer", "timeout": "5s"}
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        {"tracer": "callTracer", "timeout": "5s"}
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
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
        {"tracer": "callTracer", "timeout": "5s"}
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
            "method": "debug_traceBlockByHash",
            "params": [
                "0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e",
                {"tracer": "callTracer", "timeout": "5s"}
            ],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code title="Example JSON response" %}
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

### Response Parameters

| Field   | Type   | Description                          |
| ------- | ------ | ------------------------------------ |
| txHash  | string | Transaction hash                     |
| result  | object | Trace result for the transaction     |
| type    | string | Call type (CALL, DELEGATECALL, etc.) |
| from    | string | Sender address                       |
| to      | string | Recipient address                    |
| gasUsed | string | Gas used                             |

### Use Case

The `debug_traceBlockByHash` method is essential for:

* Block-level transaction analysis
* MEV detection and analysis
* Gas profiling across all block transactions
* Security auditing of block activity
* Chain forensics
* Protocol debugging

### Error Handling

| Status Code | Error Message   | Cause                           |
| ----------- | --------------- | ------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params  | Invalid block hash              |
| -32000      | Block not found | Block does not exist            |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceBlockByHash', [
    '0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e',
    { tracer: 'callTracer', timeout: '5s' }
]);
console.log('Block trace:', trace);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({
    method: 'debug_traceBlockByHash',
    params: [
        '0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e',
        { tracer: 'callTracer', timeout: '5s' }
    ]
});
console.log('Block trace:', trace);
```
{% endcode %}
{% endtab %}
{% endtabs %}
