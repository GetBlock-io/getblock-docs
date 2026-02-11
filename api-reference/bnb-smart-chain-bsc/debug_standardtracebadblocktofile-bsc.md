---
description: >-
  Example code for the debug_standardTraceBadBlockToFile JSON RPC method.
  Сomplete guide on how to use debug_standardTraceBadBlockToFile JSON RPC in
  GetBlock Web3 documentation.
---

# debug\_standardTraceBadBlockToFile - BSC

The `debug_standardTraceBadBlockToFile` method traces a known bad block and writes the trace to a file on the BNB Smart Chain node.

## Parameters

| Parameter | Type   | Required | Description           |
| --------- | ------ | -------- | --------------------- |
| blockHash | string | Yes      | Hash of the bad block |
| options   | object | No       | Tracer options        |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_standardTraceBadBlockToFile",
    "params": ["0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd", {}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'debug_standardTraceBadBlockToFile',
    params: ['0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd', {}],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "debug_standardTraceBadBlockToFile",
    "params": ["0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd", {}],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "debug_standardTraceBadBlockToFile",
        "params": ["0x4e3a...", {}],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": ["/tmp/block_trace_0x4e3a.jsonl"]
}
```

## Response Parameters

| Parameter | Type  | Description          |
| --------- | ----- | -------------------- |
| result    | array | File paths of traces |

## Use Cases

* Debug bad blocks
* Node diagnostics
* Chain debugging

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32602     | Invalid params       |
| -32601     | Method not supported |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const files = await provider.send('debug_standardTraceBadBlockToFile', ['0x4e3a...', {}]);
console.log(files);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const files = await client.request({
    method: 'debug_standardTraceBadBlockToFile',
    params: ['0x4e3a...', {}]
});
```
{% endtab %}
{% endtabs %}
