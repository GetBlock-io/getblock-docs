# debug\_traceBlock - Blast

This method replays and traces all transactions in an RLP-encoded block, returning per-transaction execution traces. It is a Dedicated Node tier method.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| blockRlp  | string | Yes      | 0x-prefixed RLP-encoded block    |
| options   | object | No       | Tracer options (e.g. callTracer) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlock",
    "params": ["0xf90211a0...", { "tracer": "callTracer" }],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceBlock',
    params: ['0xf90211a0...', { tracer: 'callTracer' }],
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceBlock',
        'params': ['0xf90211a0...', { 'tracer': 'callTracer' }],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "debug_traceBlock",
            "params": ["0xf90211a0...", { "tracer": "callTracer" }],
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
    "result": [
        {
            "result": {
                "type": "CALL",
                "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                "to": "0x4200000000000000000000000000000000000006",
                "gasUsed": "0x5208",
                "calls": []
            }
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | array  | Per-transaction execution traces        |

## Use Cases

* **Block Replay**: Trace every transaction in a raw block
* **Debugging**: Investigate execution across a whole block
* **Tracer Selection**: Apply callTracer or a custom tracer to a block
* **Analysis**: Reconstruct call graphs from block execution

## Error Handling

| Error Code | Message          | Description                                         |
| ---------- | ---------------- | --------------------------------------------------- |
| -32602     | Invalid params   | Malformed RLP or tracer options                     |
| -32601     | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlock', ['0xf902...', { tracer: 'callTracer' }]);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const traces = await client.request({ method: 'debug_traceBlock', params: ['0xf902...', { tracer: 'callTracer' }] });
```
{% endcode %}
{% endtab %}
{% endtabs %}
