---
description: >-
  Example code for the zks_getBytecodeByHash JSON-RPC method. Сomplete guide on
  how to use zks_getBytecodeByHash JSON-RPC in GetBlock.io Web3 documentation.
---

# zks\_getBytecodeByHash - zkSync

Returns the bytecode of a contract by its bytecode hash. zkSync stores deployed bytecode indexed by hash — multiple deployed contracts may share the same bytecode.

## Parameters

| Parameter    | Type   | Required | Description              |
| ------------ | ------ | -------- | ------------------------ |
| bytecodeHash | string | Yes      | Bytecode hash (32 bytes) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getBytecodeByHash",
    "params": [
        "0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"
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
    "method": "zks_getBytecodeByHash",
    "params": [
        "0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"
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
    "method": "zks_getBytecodeByHash",
    "params": [
        "0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"
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
        "method": "zks_getBytecodeByHash",
        "params": [
                "0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"
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
        0,
        4,
        0,
        0,
        4
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type             | Description                                                 |
| ------ | ---------------- | ----------------------------------------------------------- |
| result | array of integer | Array of uint8 values — the contract's EraVM bytecode bytes |

## Use Cases

* Contract verification flows
* Inspecting bytecode shared by multiple deployments
* Off-chain EraVM bytecode analysis

## Error Handling

| Status Code | Error Message     | Cause                               |
| ----------- | ----------------- | ----------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>` |
| 429         | Too Many Requests | Rate limit exceeded for your plan   |

## SDK Integration

{% tabs %}
{% tab title="zksync-ethers (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { Provider } from 'zksync-ethers';

const provider = new Provider('https://go.getblock.io/<ACCESS-TOKEN>/');

// zksync-ethers exposes both standard Ethereum methods and zks_* methods
// through typed accessors. For raw access:
const result = await provider.send('zks_getBytecodeByHash', ["0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"]);
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
result = zk_web3.zksync._zks_endpoints if 'zks_getBytecodeByHash'.startswith('zks_') else None
# Or call any method generically:
result = zk_web3.provider.make_request('zks_getBytecodeByHash', ["0x0100067d861e2f5717a12c3e869cfb657793b86bbb0caa05cc1421f16c5217bc"])
print(result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
