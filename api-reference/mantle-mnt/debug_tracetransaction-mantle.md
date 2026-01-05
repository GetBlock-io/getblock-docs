---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - Mantle

This method returns a full trace of a transaction execution. It allows developers to debug and analyze transaction behavior at the EVM level.

### Parameters

| Parameter       | Type   | Description                             |
| --------------- | ------ | --------------------------------------- |
| transactionHash | string | Hash of the transaction to trace        |
| tracerConfig    | object | (optional) Tracer configuration options |

**Tracer Config Options:**

| Field       | Type    | Description                                       |
| ----------- | ------- | ------------------------------------------------- |
| tracer      | string  | Type of tracer ("callTracer" or "prestateTracer") |
| timeout     | string  | Timeout for the trace operation                   |
| onlyTopCall | boolean | Only trace the main call, not sub-calls           |

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="debug_traceTransaction.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
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
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="debug_traceTransaction.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
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
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="debug_traceTransaction.rs" %}
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
                "0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "type": "CALL",
        "from": "0x52988d3dd2d1b36e9c48866670b7683c42197139",
        "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
        "value": "0xde0b6b3a7640000",
        "gas": "0x5208",
        "gasUsed": "0x5208",
        "input": "0x",
        "output": "0x"
    }
}
```

### Response Parameters

| Field   | Type   | Description                          |
| ------- | ------ | ------------------------------------ |
| type    | string | Call type (CALL, DELEGATECALL, etc.) |
| from    | string | Sender address                       |
| to      | string | Recipient address                    |
| value   | string | Value transferred (hex)              |
| gas     | string | Gas provided (hex)                   |
| gasUsed | string | Gas used (hex)                       |
| input   | string | Call input data                      |
| output  | string | Call output data                     |
| calls   | array  | Sub-calls made during execution      |

### Use Case

The `debug_traceTransaction` method is essential for:

* Transaction debugging and analysis
* Smart contract debugging
* Gas optimization analysis
* Understanding internal calls
* Security auditing

### Error Handling

| Status Code | Error Message         | Cause                           |
| ----------- | --------------------- | ------------------------------- |
| 403         | Forbidden             | Missing or invalid ACCESS-TOKEN |
| -32000      | Transaction not found | Invalid transaction hash        |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-debug_traceTransaction.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', [
    '0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8',
    { tracer: 'callTracer' }
]);
console.log('Transaction Trace:', trace);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-debug_traceTransaction.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({
    method: 'debug_traceTransaction',
    params: [
        '0x6dfef681e0a83a40b05d40877e53a88459e8829240a6d6d5ec6fe816e435f8d8',
        { tracer: 'callTracer' }
    ]
});
console.log('Transaction Trace:', trace);
```
{% endcode %}
{% endtab %}
{% endtabs %}
