---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Сomplete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock.io Web3 documentation.
---

# debug\_traceBlockByHash - zkSync

Traces the execution of all transactions within a block specified by hash. Returns an array of trace results, one per transaction.

## Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| blockHash | string | Yes      | 32-byte hash of the block |
| options   | object | No       | Optional tracer options   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        {
            "tracer": "callTracer",
            "timeout": "120s"
        }
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        {
            "tracer": "callTracer",
            "timeout": "120s"
        }
    ],
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
        {
            "tracer": "callTracer",
            "timeout": "120s"
        }
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "debug_traceBlockByHash",
        "params": [
                "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
                {
                        "tracer": "callTracer",
                        "timeout": "120s"
                }
        ],
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "result": {
                "type": "Call",
                "from": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
                "to": "0xd85498dbeaeb1df24be52eed4f52eac2fbd56245",
                "gas": "0x18be25",
                "gasUsed": "0x5208",
                "value": "0x0",
                "output": "0x",
                "input": "0x",
                "calls": []
            }
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field            | Type            | Description                      |
| ---------------- | --------------- | -------------------------------- |
| result           | array of object | Per-transaction trace results    |
| result\[].result | object          | Trace object for one transaction |

## Use Cases

* Block-wide debugging and analytics
* Forensic analysis of a specific block
* Computing per-block call-graph statistics

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('debug_traceBlockByHash', ["0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b", {"tracer": "callTracer", "timeout": "120s"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="zksync2-python (Python)" %}
{% code overflow="wrap" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

# zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'debug_traceBlockByHash'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('debug_traceBlockByHash', ["0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b", {"tracer": "callTracer", "timeout": "120s"}])
print(result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
