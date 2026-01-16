---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide on how to use
  eth_syncing JSON-RPC in GetBlock Web3 documentation
---

# eth\_syncing - Somnia

This method returns the sync status of the Somnia node. If the node is fully synced, it returns false. If syncing, it returns an object with sync progress details.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_syncing",
  "params": []
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_syncing',
  params: []
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
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_syncing",
    "params": []
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
        "id": "getblock.io",
        "method": "eth_syncing",
        "params": []
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

## Response Example (Synced)

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": false
}
```

## Response Example (Syncing)

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "startingBlock": "0x0",
    "currentBlock": "0x1a2b3c",
    "highestBlock": "0x1a2b40"
  }
}
```

## Response Parameters

| Parameter     | Type   | Description              |
| ------------- | ------ | ------------------------ |
| startingBlock | string | Block where sync started |
| currentBlock  | string | Current synced block     |
| highestBlock  | string | Estimated highest block  |

## Use Cases

* Check node readiness
* Monitor sync progress
* Health checks for applications
* Validate data reliability

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncStatus = await provider.send('eth_syncing', []);
console.log(syncStatus);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const syncStatus = await client.request({
  method: 'eth_syncing',
  params: []
});
console.log(syncStatus);
```
{% endtab %}
{% endtabs %}
