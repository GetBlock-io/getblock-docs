---
description: >-
  Example code for the eth_simulateV1 JSON-RPC method. Complete guide on how to
  use eth_simulateV1 JSON-RPC in GetBlock Web3 documentation.
---

# eth\_simulateV1 - Arbitrum Nova

This method simulates one or more blocks of transactions against a base block, returning the resulting state changes, return values, and logs. It is used to preview multi-step interactions before broadcasting.

## Parameters

| Parameter      | Type   | Required | Description                                         |
| -------------- | ------ | -------- | --------------------------------------------------- |
| payload        | object | Yes      | Simulation payload with blockStateCalls and options |
| blockParameter | string | Yes      | Base block in hex, or "latest", "pending"           |

### Payload Object

| Field           | Type    | Required | Description                                                               |
| --------------- | ------- | -------- | ------------------------------------------------------------------------- |
| blockStateCalls | array   | Yes      | Array of block objects each containing calls and optional state overrides |
| validation      | boolean | No       | Whether to run full validation on each call                               |
| traceTransfers  | boolean | No       | Whether to emit synthetic transfer logs                                   |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_simulateV1",
    "params": [{ "blockStateCalls": [{ "calls": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }] }], "validation": true, "traceTransfers": true }, "latest"],
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
    method: 'eth_simulateV1',
    params: [{ blockStateCalls: [{ calls: [{ from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0x4200000000000000000000000000000000000006', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }] }], validation: true, traceTransfers: true }, 'latest'],
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
        'method': 'eth_simulateV1',
        'params': [{ 'blockStateCalls': [{ 'calls': [{ 'from': '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', 'to': '0x4200000000000000000000000000000000000006', 'data': '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' }] }], 'validation': True, 'traceTransfers': True }, 'latest'],
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
            "method": "eth_simulateV1",
            "params": [{ "blockStateCalls": [{ "calls": [{ "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0x4200000000000000000000000000000000000006", "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045" }] }], "validation": true, "traceTransfers": true }, "latest"],
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
            "number": "0x14b8a10",
            "calls": [
                {
                    "status": "0x1",
                    "returnData": "0x00",
                    "gasUsed": "0x5b8d",
                    "logs": []
                }
            ]
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")              |
| id        | string | Request identifier matching the request        |
| result    | array  | Per-simulated-block results with call outcomes |

## Use Cases

* **Bundle Preview**: Simulate a sequence of calls to preview outcomes
* **State Overrides**: Test calls against modified balances or storage
* **Transfer Tracing**: Capture synthetic transfer logs for value flows
* **dApp Safety**: Warn users about failing or unexpected results before signing

## Error Handling

| Error Code | Message         | Description                              |
| ---------- | --------------- | ---------------------------------------- |
| -32602     | Invalid params  | Malformed simulation payload             |
| -32000     | Execution error | A simulated call failed during execution |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const sim = await provider.send('eth_simulateV1', [payload, 'latest']);
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

const sim = await client.simulateBlocks({ blocks: payload.blockStateCalls });
```
{% endcode %}
{% endtab %}
{% endtabs %}
