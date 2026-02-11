---
description: >-
  Example code for the eth_blockNumber JSON RPC method. Сomplete guide on how to
  use eth_blockNumber JSON RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - BSC

This method returns the number of the most recent block on the BNB Smart Chain. This is a fundamental method for tracking blockchain state and monitoring network progress. With BSC's \~3 second block times, this value updates frequently, making it essential for applications requiring real-time blockchain data.

## Parameter

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
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
    method: 'eth_blockNumber',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const blockNumber = parseInt(response.data.result, 16);
    console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
block_number = int(response.json()["result"], 16)
print(f"Current Block Number: {block_number}")
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
        "method": "eth_blockNumber",
        "params": [],
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
    "result": "0x4a88c98"
}
```

## Response Parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC version (2.0)                                  |
| id        | string | Request identifier                                      |
| result    | string | Hexadecimal block number (e.g., 0x2625a00 = 40,000,000) |

## Use Cases

* Monitor blockchain synchronization status
* Calculate transaction confirmations
* Track network liveness and progress
* Implement block-based polling for updates
* Determine finality for transactions on BSC

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await provider.getBlockNumber();
console.log('Current Block:', blockNumber);
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

const blockNumber = await client.getBlockNumber();
console.log('Current Block:', blockNumber);
```
{% endtab %}
{% endtabs %}
