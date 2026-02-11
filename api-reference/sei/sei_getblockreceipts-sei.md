---
description: >-
  Example code for the sei_getBlockReceipts JSON-RPC method. Complete guide on
  how to use sei_getBlockReceipts JSON-RPC in GetBlock Web3 documentation
---

# sei\_getBlockReceipts - SEI

Returns all transaction receipts for a block on the Sei network, including both EVM transactions and synthetic transactions from Cosmos. Unlike eth\_getBlockReceipts, this includes Cosmos transaction receipts.

## Parameters

| Parameter   | Type   | Description                                                                  |
| ----------- | ------ | ---------------------------------------------------------------------------- |
| blockNumber | string | Block number in hex, or 'latest', 'earliest', 'pending', 'safe', 'finalized' |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "sei_getBlockReceipts",
    "params": ["latest"],
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
    method: 'sei_getBlockReceipts',
    params: ["latest"],
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
    "method": "sei_getBlockReceipts",
    "params": ["latest"],
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
            "method": "sei_getBlockReceipts",
            "params": ["latest"],
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
            "transactionHash": "0x...",
            "blockNumber": "0x4B8F2A1",
            "gasUsed": "0x5208",
            "status": "0x1",
            "logs": [...],
            "synthetic": false
        },
        {
            "transactionHash": "0x...",
            "blockNumber": "0x4B8F2A1",
            "synthetic": true
        }
    ]
}
```

## Response Parameters

| Field     | Type    | Description                                                           |
| --------- | ------- | --------------------------------------------------------------------- |
| result    | array   | Array of transaction receipt objects including synthetic transactions |
| synthetic | boolean | Indicates if the transaction is synthetic (from Cosmos)               |

## Use Case

The `sei_getBlockReceipts` method is essential for:

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
const result = await provider.send('sei_getBlockReceipts', ["latest"]);
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
    method: 'sei_getBlockReceipts',
    params: ["latest"]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
