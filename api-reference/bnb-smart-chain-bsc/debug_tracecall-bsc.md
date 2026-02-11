---
description: >-
  Example code for the debug_traceCall JSON RPC method. Сomplete guide on how to
  use debug_traceCall JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceCall - BSC

The `debug_traceCall` method lets you run `eth_call` with tracing on the BNB Smart Chain. This is useful for simulating and debugging contract calls without sending a transaction.

## Parameters

| Parameter   | Type   | Required | Description              |
| ----------- | ------ | -------- | ------------------------ |
| transaction | object | Yes      | Transaction call object  |
| blockNumber | string | Yes      | Block number or "latest" |
| options     | object | No       | Tracer options           |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [{
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    }, "latest", {"tracer": "callTracer"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_traceCall',
    params: [{
        to: '0x55d398326f99059fF775485246999027B3197955',
        data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45'
    }, 'latest', { tracer: 'callTracer' }],
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
{% code title="request.py" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [{
        "to": "0x55d398326f99059fF775485246999027B3197955",
        "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    }, "latest", {"tracer": "callTracer"}],
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
        "method": "debug_traceCall",
        "params": [{
            "to": "0x55d398326f99059fF775485246999027B3197955",
            "data": "0x70a08231..."
        }, "latest", {"tracer": "callTracer"}],
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

{% code title="response.json" overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "from": "0x0000000000000000000000000000000000000000",
        "gas": "0x17d7840",
        "gasUsed": "0x5d9b",
        "to": "0x55d398326f99059ff775485246999027b3197955",
        "input": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "output": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "value": "0x0",
        "type": "CALL"
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description  |
| --------- | ------ | ------------ |
| type      | string | Call type    |
| gasUsed   | string | Gas consumed |
| output    | string | Return data  |

## Use Cases

* Simulate contract calls
* Debug before sending transactions
* Analyze call traces
* Test contract interactions

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32602     | Invalid params |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceCall', [{
    to: '0x55d398326f99059fF775485246999027B3197955',
    data: '0x70a08231...'
}, 'latest', { tracer: 'callTracer' }]);
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
    method: 'debug_traceCall',
    params: [{ to: '0x55d398...', data: '0x70a08231...' }, 'latest', { tracer: 'callTracer' }]
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
