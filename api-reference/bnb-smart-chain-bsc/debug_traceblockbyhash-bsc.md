---
description: >-
  Example code for the debug_traceBlockByHash JSON RPC method. Сomplete guide on
  how to use debug_traceBlockByHash JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceBlockByHash - BSC

The `debug_traceBlockByHash` method returns traces for all transactions in a block identified by its hash on the BNB Smart Chain.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| blockHash | string | Yes      | 32-byte block hash |
| options   | object | No       | Tracer options     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": ["0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd", {"tracer": "callTracer"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_traceBlockByHash',
    params: ['0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd', { tracer: 'callTracer' }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": ["0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd", {"tracer": "callTracer"}],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "debug_traceBlockByHash",
        "params": ["0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd", {"tracer": "callTracer"}],
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

## Response Example

{% code title="Response (JSON)" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [{
        "result": {
            "type": "CALL",
            "from": "0x...",
            "to": "0x...",
            "gasUsed": "0x5208"
        }
    }]
}
```
{% endcode %}

## Response Parameters

| Parameter | Type  | Description            |
| --------- | ----- | ---------------------- |
| result    | array | Array of trace results |

## Use Cases

* Debug specific block execution
* Analyze block transactions
* Build block explorers with traces

## Error Handling

| Error Code | Description     |
| ---------- | --------------- |
| -32602     | Invalid params  |
| -32000     | Block not found |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlockByHash', [
    '0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd',
    { tracer: 'callTracer' }
]);
console.log(traces);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const traces = await client.request({
    method: 'debug_traceBlockByHash',
    params: ['0x4e3a375...', { tracer: 'callTracer' }]
})
```
{% endcode %}
{% endtab %}
{% endtabs %}
