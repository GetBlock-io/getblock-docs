---
description: >-
  Example code for the debug_traceCall JSON-RPC method. Complete guide on how to
  use debug_traceCall JSON-RPC in GetBlock Web3 documentation.
---

# debug\_tracecall - Worldchain

Traces a call without creating a transaction on the World Chain network for debugging purposes.

### Parameters

| Parameter    | Type   | Description                                             |
| ------------ | ------ | ------------------------------------------------------- |
| transaction  | object | Transaction call object                                 |
| blockNumber  | string | Block number in hex, or "latest", "earliest", "pending" |
| tracerConfig | object | (Optional) Tracer configuration options                 |

### Request

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x..."}, "latest", {}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x..."}, "latest", {}],
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
    "method": "debug_traceCall",
    "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x..."}, "latest", {}],
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
            "method": "debug_traceCall",
            "params": [{"to": "0x1234567890123456789012345678901234567890", "data": "0x..."}, "latest", {}],
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
    "result": {
        "gas": 21000,
        "structLogs": [...]
    }
}
```

### Response Parameters

| Field      | Type   | Description                             |
| ---------- | ------ | --------------------------------------- |
| gas        | number | Gas used by the call                    |
| structLogs | array  | Array of structured logs from execution |

### Use Case

The `debug_traceCall` method on World Chain is typically used for:

* Call simulation
* Gas estimation debugging
* Contract call analysis
* Pre-execution verification

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

const trace = await provider.send('debug_traceCall', [{
    to: '0x1234567890123456789012345678901234567890',
    data: '0x...'
}, 'latest', {}]);
console.log('Trace:', trace);
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

const trace = await client.request({
    method: 'debug_traceCall',
    params: [{
        to: '0x1234567890123456789012345678901234567890',
        data: '0x...'
    }, 'latest', {}]
});
console.log('Trace:', trace);
```
{% endtab %}
{% endtabs %}
