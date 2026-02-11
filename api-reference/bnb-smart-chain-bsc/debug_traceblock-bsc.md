---
description: >-
  Example code for the debug_traceBlock JSON RPC method. Сomplete guide on how
  to use debug_traceBlock JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceBlock - BSC

The `debug_traceBlock` method returns traces for all transactions in a block given the block's RLP-encoded bytes on the BNB Smart Chain.

## Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| blockRlp  | string | Yes      | RLP-encoded block data |
| options   | object | No       | Tracer options         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlock",
    "params": ["0xf90...", {"tracer": "callTracer"}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_traceBlock',
    params: ['0xf90...', { tracer: 'callTracer' }],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "debug_traceBlock",
    "params": ["0xf90...", {"tracer": "callTracer"}],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "debug_traceBlock",
        "params": ["0xf90...", {"tracer": "callTracer"}],
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
{% endtab %}
{% endtabs %}

## Response Example

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

## Response Parameters

| Parameter | Type  | Description            |
| --------- | ----- | ---------------------- |
| result    | array | Array of trace results |

## Use Cases

* Analyze entire block execution
* Debug block-level issues
* Audit block transactions

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlock', ['0xf90...', { tracer: 'callTracer' }]);
console.log(traces);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const traces = await client.request({
    method: 'debug_traceBlock',
    params: ['0xf90...', { tracer: 'callTracer' }]
});
```
{% endtab %}
{% endtabs %}
