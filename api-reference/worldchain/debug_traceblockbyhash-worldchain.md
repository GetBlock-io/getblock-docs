---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Complete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceblockbyhash - Worldchain

Returns all traces of all transactions in a block by block hash on the World Chain network.

### Parameters

| Parameter    | Type   | Description                             |
| ------------ | ------ | --------------------------------------- |
| blockHash    | string | Hash of the block to trace              |
| tracerConfig | object | (Optional) Tracer configuration options |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", {}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", {}],
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

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", {}],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
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
            "params": ["0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e", {}],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "txHash": "0x...",
            "result": {...}
        }
    ]
}
```

### Response Parameters

| Field  | Type  | Description                                              |
| ------ | ----- | -------------------------------------------------------- |
| result | array | Array of trace results for each transaction in the block |

### Use Case

The `debug_traceBlockByHash` method on World Chain is typically used for:

* Block analysis
* Transaction debugging
* Execution tracing
* Historical analysis

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlockByHash', ['0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e', {}]);
console.log('Traces:', traces);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { worldchain } from 'viem/chains';

const client = createPublicClient({
    chain: worldchain,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const traces = await client.request({
    method: 'debug_traceBlockByHash',
    params: ['0x9b14d73f45c836bfb0e1f59453c39fe8cddab45a0f78670dc6190e5c85b65a8e', {}]
});
console.log('Traces:', traces);
```
{% endtab %}
{% endtabs %}
