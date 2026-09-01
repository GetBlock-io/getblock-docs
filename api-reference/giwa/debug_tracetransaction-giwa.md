---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - GIWA

This method replays a mined transaction and returns a detailed execution trace, including internal calls and value transfers. It is a Dedicated Node tier method widely used for debugging and MEV analysis.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| transactionHash | string | Yes | 32-byte transaction hash |
| options | object | No | Tracer options (e.g. callTracer, prestateTracer) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}

```bash
curl --location --request POST 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234", { "tracer": "callTracer" }],
    "id": "getblock.io"
}'
```

{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}

```javascript
const axios = require('axios');

const response = await axios.post('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceTransaction',
    params: ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234', { tracer: 'callTracer' }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```

{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}

```python
import requests

response = requests.post(
    'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceTransaction',
        'params': ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234', { 'tracer': 'callTracer' }],
        'id': 'getblock.io'
    }
)

print(response.json())
```

{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}

```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "debug_traceTransaction",
            "params": ["0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234", { "tracer": "callTracer" }],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```

{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "type": "CALL",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0x4200000000000000000000000000000000000006",
        "value": "0x0",
        "gasUsed": "0x5208",
        "output": "0x",
        "calls": []
    }
}
```

## Response Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| jsonrpc | string | JSON-RPC protocol version ("2.0") |
| id | string | Request identifier matching the request |
| result | object | Execution trace of the transaction |

## Use Cases

* **Transaction Debugging**: Trace exactly how a transaction executed
* **Internal Transfers**: Extract nested calls and ETH movements
* **MEV Analysis**: Reconstruct arbitrage and sandwich behavior
* **Revert Root Cause**: Locate the frame where a transaction reverted

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32602 | Invalid params | Malformed transaction hash or tracer options |
| -32601 | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}

```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234', { tracer: 'callTracer' }]);
```

{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}

```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({ method: 'debug_traceTransaction', params: ['0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234', { tracer: 'callTracer' }] });
```

{% endcode %}
{% endtab %}
{% endtabs %}
