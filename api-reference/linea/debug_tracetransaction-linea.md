---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - Linea

This method replays a transaction and returns a detailed execution trace, including opcode-level steps or a structured call trace depending on the tracer. It is used to debug transaction execution and reverts.

## Parameters

| Parameter       | Type   | Required | Description                                                   |
| --------------- | ------ | -------- | ------------------------------------------------------------- |
| transactionHash | string | Yes      | 32-byte hash of the transaction to trace                      |
| options         | object | No       | Tracer options, such as the tracer name and its configuration |

### Options Object

| Field   | Type   | Required | Description                                         |
| ------- | ------ | -------- | --------------------------------------------------- |
| tracer  | string | No       | Tracer to use, such as callTracer or prestateTracer |
| timeout | string | No       | Maximum trace duration, such as "10s"               |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b", {"tracer": "callTracer"}],
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
    method: 'debug_traceTransaction',
    params: ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b", {"tracer": "callTracer"}],
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
        'method': 'debug_traceTransaction',
        'params': ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b", {"tracer": "callTracer"}],
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
            "method": "debug_traceTransaction",
            "params": ["0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b", {"tracer": "callTracer"}],
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
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f",
        "value": "0xde0b6b3a7640000",
        "gas": "0x5208",
        "gasUsed": "0x5208",
        "input": "0x",
        "output": "0x",
        "type": "CALL",
        "calls": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | object | Execution trace, shaped by the selected tracer |

### callTracer Object

| Field   | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| from    | string | Address that initiated the call |
| to      | string | Address that received the call  |
| gasUsed | string | Gas used by the call frame      |
| output  | string | Return data of the call         |
| calls   | array  | Nested internal calls           |

## Use Cases

* **Revert Debugging**: Trace why a transaction reverted
* **Internal Calls**: Reconstruct internal contract calls with callTracer
* **MEV Analysis**: Inspect execution paths of complex transactions
* **Gas Profiling**: Attribute gas usage across call frames

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | The transaction hash is not a valid 32-byte hex string |
| -32603     | Internal error | The node failed to read the transaction                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', ['0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b', { tracer: 'callTracer' }]);
console.log(trace);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const trace = await client.request({ method: 'debug_traceTransaction', params: ['0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b', { tracer: 'callTracer' }] });
console.log(trace);
```
{% endcode %}
{% endtab %}
{% endtabs %}
