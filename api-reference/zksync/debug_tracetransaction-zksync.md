---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Сomplete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock.io Web3 documentation.
---

# debug\_traceTransaction - zkSync

Replays a transaction and returns a detailed execution trace. Use the optional second parameter to specify a tracer (e.g. `callTracer`) and timeout.

## Parameters

| Parameter | Type   | Required | Description                                                             |
| --------- | ------ | -------- | ----------------------------------------------------------------------- |
| txHash    | string | Yes      | Transaction hash to trace                                               |
| options   | object | No       | Optional tracer options, e.g. `{tracer: 'callTracer', timeout: '120s'}` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceTransaction",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
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
    "method": "debug_traceTransaction",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
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
    "method": "debug_traceTransaction",
    "params": [
        "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
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
        "method": "debug_traceTransaction",
        "params": [
                "0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9",
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
    "result": {
        "type": "Call",
        "from": "0x36615cf349d7f6344891b1e7ca7c72883f5dc049",
        "to": "0x0000000000000000000000000000000000008001",
        "gas": "0x18be25",
        "gasUsed": "0x7603b",
        "value": "0xa1e94fc0fe6043",
        "output": "0x",
        "input": "0x",
        "error": null,
        "revertReason": null,
        "calls": []
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type           | Description                                        |
| ------------------- | -------------- | -------------------------------------------------- |
| result.type         | string         | Call type (`Call`, `Create`, `DelegateCall`, etc.) |
| result.from         | string         | Caller address                                     |
| result.to           | string         | Callee address                                     |
| result.gas          | string         | Gas provided to the call (hex)                     |
| result.gasUsed      | string         | Gas actually consumed by the call (hex)            |
| result.value        | string         | ETH value attached to the call (wei, hex)          |
| result.input        | string         | Calldata                                           |
| result.output       | string         | Return data                                        |
| result.calls        | array          | Recursive list of sub-calls made during execution  |
| result.error        | string \| null | Error message if the call failed                   |
| result.revertReason | string \| null | Decoded revert reason, if available                |

## Use Cases

* Debugging failed or reverted transactions
* Per-call gas analysis
* Decoding internal contract calls for analytics

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
const result = await provider.send('debug_traceTransaction', ["0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9", {"tracer": "callTracer", "timeout": "120s"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="zksync2-python (Python)" %}
{% code overflow="wrap" %}
```python
from zksync2.module.module_builder import ZkSyncBuilder

zk_web3 = ZkSyncBuilder.build('https://go.getblock.io/<ACCESS-TOKEN>/')

// zksync2-python exposes the JSON-RPC layer directly.
result = zk_web3.zksync._zks_endpoints if 'debug_traceTransaction'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('debug_traceTransaction', ["0x22de7debaa98758afdaee89f447ff43bab5da3de6acca7528b281cc2f1be2ee9", {"tracer": "callTracer", "timeout": "120s"}])
print(result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
