---
description: >-
  Example code for the sei_traceBlockByNumberExcludeTraceFail JSON-RPC method.
  Complete guide on how to use sei_traceBlockByNumberExcludeTraceFail JSON-RPC
  in GetBlock Web3 documentation.
---

# sei\_traceBlockByNumberExcludeTraceFail - SEI

This method traces all transactions in a block on Sei by block number, excluding transactions that fail tracing. Provides cleaner debugging output by filtering out failed traces.

## Parameters

| Parameter   | Type   | Description                                                                  |
| ----------- | ------ | ---------------------------------------------------------------------------- |
| blockNumber | string | Block number in hex, or 'latest', 'earliest', 'pending', 'safe', 'finalized' |
| options     | object | Tracing options including 'tracer', 'timeout', etc. (optional)               |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "sei_traceBlockByNumberExcludeTraceFail",
    "params": ["latest", {"tracer": "callTracer"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'sei_traceBlockByNumberExcludeTraceFail',
    params: ["latest", {"tracer": "callTracer"}],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "sei_traceBlockByNumberExcludeTraceFail",
    "params": ["latest", {"tracer": "callTracer"}],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust (reqwest)" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "sei_traceBlockByNumberExcludeTraceFail",
            "params": ["latest", {"tracer": "callTracer"}],
            "id": "getblock.io"
        }))
        .send()
        .await?;
    
    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "result": {
                "type": "CALL",
                "from": "0x...",
                "to": "0x...",
                "gasUsed": "0x5208"
            }
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type  | Description                                                      |
| ------ | ----- | ---------------------------------------------------------------- |
| result | array | Array of trace results for successfully traced transactions only |

## Use Case

The `sei_traceBlockByNumberExcludeTraceFail` method is essential for:

* Blockchain developers building applications on Sei
* Wallet applications requiring network data
* Analytics platforms tracking Sei network activity
* DeFi protocols integrating with Sei's parallelized EVM

## Error Handling

Common errors when using this method:

| Error Code | Message          | Description                                |
| ---------- | ---------------- | ------------------------------------------ |
| -32700     | Parse error      | Invalid JSON                               |
| -32600     | Invalid Request  | JSON is not a valid request object         |
| -32601     | Method not found | Method does not exist                      |
| -32602     | Invalid params   | Invalid method parameters                  |
| -32603     | Internal error   | Internal JSON-RPC error                    |
| -32000     | Invalid input    | Generic input error                        |
| -32500     | Cross-VM error   | Error in cross-VM operation (Sei-specific) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using ethers provider methods
const result = await provider.send('sei_traceBlockByNumberExcludeTraceFail', ["latest", {"tracer": "callTracer"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sei } from 'viem/chains';

const client = createPublicClient({
    chain: sei,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using viem's request method
const result = await client.request({
    method: 'sei_traceBlockByNumberExcludeTraceFail',
    params: ["latest", {"tracer": "callTracer"}]
});
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
