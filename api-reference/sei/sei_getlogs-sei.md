---
description: >-
  Example code for the sei_getLogs JSON-RPC method. Complete guide on how to use
  sei_getLogs JSON-RPC in GetBlock Web3 documentation
---

# sei\_getLogs - SEI

Returns logs matching the filter criteria on Sei, including synthetic logs generated from Cosmos events. This extends eth\_getLogs to support cross-VM event tracking.

## Parameters

| Parameter | Type   | Description                                                           |
| --------- | ------ | --------------------------------------------------------------------- |
| filter    | object | Filter object with 'fromBlock', 'toBlock', 'address', 'topics' fields |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "sei_getLogs",
    "params": [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'sei_getLogs',
    params: [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "sei_getLogs",
    "params": [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
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
            "method": "sei_getLogs",
            "params": [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}],
            "id": "getblock.io"
        }))
        .send()
        .await?;
    
    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
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
    "result": [
        {
            "address": "0x123456...",
            "topics": ["0xddf252ad..."],
            "data": "0x...",
            "blockNumber": "0x4B8F2A1",
            "synthetic": true
        }
    ]
}
```

## Response Parameters

| Field     | Type    | Description                                     |
| --------- | ------- | ----------------------------------------------- |
| result    | array   | Array of log objects including synthetic logs   |
| synthetic | boolean | Indicates if the log is synthetic (from Cosmos) |

## Use Case

The `sei_getLogs` method is essential for:

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
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using ethers provider methods
const result = await provider.send('sei_getLogs', [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sei } from 'viem/chains';

const client = createPublicClient({
    chain: sei,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using viem's request method
const result = await client.request({
    method: 'sei_getLogs',
    params: [{"fromBlock": "0x4B8F000", "toBlock": "latest", "address": "0x1234567890abcdef1234567890abcdef12345678"}]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
