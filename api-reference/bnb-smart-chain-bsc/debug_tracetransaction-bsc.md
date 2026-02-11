---
description: >-
  Example code for the debug_traceTransaction JSON RPC method. Сomplete guide on
  how to use debug_traceTransaction JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - BSC

The `debug_traceTransaction` method returns a detailed trace of a transaction's execution on the BNB Smart Chain. This is useful for debugging smart contract interactions and understanding gas consumption.

### Parameters

| Parameter       | Type   | Required | Description               |
| --------------- | ------ | -------- | ------------------------- |
| transactionHash | string | Yes      | Transaction hash to trace |
| options         | object | No       | Tracer options            |

#### Options Object

| Field   | Type   | Description                              |
| ------- | ------ | ---------------------------------------- |
| tracer  | string | Tracer type (callTracer, prestateTracer) |
| timeout | string | Execution timeout                        |

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_traceTransaction',
    params: [
        '0x88df016429689c079f3b2f6ad39fa052532c56795f8bb45',
        { tracer: 'callTracer' }
    ],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        {"tracer": "callTracer"}
    ],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "debug_traceTransaction",
        "params": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            {"tracer": "callTracer"}
        ],
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "type": "CALL",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "to": "0x55d398326f99059ff775485246999027b3197955",
        "value": "0x0",
        "gas": "0x5208",
        "gasUsed": "0x5208",
        "input": "0xa9059cbb...",
        "output": "0x"
    }
}
```
{% endcode %}

### Response Parameters

| Parameter | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| type      | string | Call type (CALL, DELEGATECALL, etc.) |
| from      | string | Caller address                       |
| to        | string | Called address                       |
| gasUsed   | string | Gas consumed                         |

### Use Cases

* Debug failed transactions
* Analyze internal calls
* Audit smart contract execution
* Optimize gas usage

### Error Handling

| Error Code | Description           |
| ---------- | --------------------- |
| -32602     | Invalid params        |
| -32000     | Transaction not found |
| -32603     | Internal error        |

### SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', [
    '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b',
    { tracer: 'callTracer' }
]);
console.log(trace);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({
    method: 'debug_traceTransaction',
    params: ['0x88df...', { tracer: 'callTracer' }]
});
console.log(trace);
```
{% endcode %}
{% endtab %}
{% endtabs %}
