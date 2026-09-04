---
description: >-
  Example code for the debug_traceCall JSON-RPC method. Complete guide on how to
  use debug_traceCall JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceCall - Moonbeam

This method traces a simulated message call against a given block without submitting a transaction, returning a detailed execution trace. It is a Dedicated Node tier method.

{% hint style="warning" %}
This method belongs to the `debug` namespace and is available on GetBlock **Dedicated Nodes** only. It is not served on shared endpoints.
{% endhint %}

## Parameters

| Parameter      | Type   | Required | Description                      |
| -------------- | ------ | -------- | -------------------------------- |
| transaction    | object | Yes      | Transaction call object          |
| blockParameter | string | Yes      | Block number in hex, or "latest" |
| options        | object | No       | Tracer options (e.g. callTracer) |

### Transaction Object

| Field | Type   | Required | Description                        |
| ----- | ------ | -------- | ---------------------------------- |
| from  | string | No       | 20-byte sender address             |
| to    | string | Yes      | 20-byte recipient/contract address |
| data  | string | No       | Encoded function call data         |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xAcc15dC74880C9944775448304B263D191c6077F", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest", { "tracer": "callTracer" }],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceCall',
    params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xAcc15dC74880C9944775448304B263D191c6077F', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest', { tracer: 'callTracer' }],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceCall',
        'params': [{ 'from': '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'to': '0xAcc15dC74880C9944775448304B263D191c6077F', 'data': '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest', { 'tracer': 'callTracer' }],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "debug_traceCall",
            "params": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xAcc15dC74880C9944775448304B263D191c6077F", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }, "latest", { "tracer": "callTracer" }],
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
        "to": "0xAcc15dC74880C9944775448304B263D191c6077F",
        "gasUsed": "0x5208",
        "output": "0x",
        "calls": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Execution trace of the simulated call   |

## Use Cases

* **Call Debugging**: Trace a read or write call without broadcasting it
* **Revert Analysis**: See where a failing call reverts
* **Internal Transfers**: Extract nested calls and value flows
* **dApp Diagnostics**: Investigate contract behavior during development

## Error Handling

| Error Code | Message          | Description                                         |
| ---------- | ---------------- | --------------------------------------------------- |
| -32602     | Invalid params   | Malformed transaction, block parameter, or options  |
| -32601     | Method not found | The debug namespace is not enabled on this endpoint |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceCall', [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xAcc15dC74880C9944775448304B263D191c6077F', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest', { tracer: 'callTracer' }]);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({ method: 'debug_traceCall', params: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xAcc15dC74880C9944775448304B263D191c6077F', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }, 'latest', { tracer: 'callTracer' }] });
```
{% endtab %}
{% endtabs %}
