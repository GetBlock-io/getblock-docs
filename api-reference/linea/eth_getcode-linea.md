---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Linea

This method returns the compiled bytecode deployed at an address. An externally owned account returns `0x`, while a contract returns its runtime bytecode.

## Parameters

| Parameter      | Type   | Required | Description                                             |
| -------------- | ------ | -------- | ------------------------------------------------------- |
| address        | string | Yes      | 20-byte address to read code from                       |
| blockParameter | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getCode',
    params: ["0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f", "latest"],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getCode',
        'params': ["0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f", "latest"],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getCode",
            "params": ["0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f", "latest"],
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
    "result": "0x6080604052600436106101c25760003560e01c8063..."
}
```

## Response Parameters

| Parameter | Type   | Description                                                    |
| --------- | ------ | -------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                              |
| id        | string | Request identifier matching the request                        |
| result    | string | Hex-encoded runtime bytecode, or 0x for a non-contract account |

## Use Cases

* **Contract Detection**: Distinguish a contract from an externally owned account
* **Bytecode Verification**: Compare deployed bytecode against a compiled artifact
* **Proxy Inspection**: Read the runtime code behind a proxy address
* **Security Analysis**: Retrieve bytecode for static analysis

## Error Handling

| Error Code | Message        | Description                                                                |
| ---------- | -------------- | -------------------------------------------------------------------------- |
| -32602     | Invalid params | The address is not a valid 20-byte hex string, or the block tag is invalid |
| -32603     | Internal error | The node failed to read the requested state                                |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f');
console.log(code);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const code = await client.getBytecode({ address: '0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f' });
console.log(code);
```
{% endcode %}
{% endtab %}
{% endtabs %}
