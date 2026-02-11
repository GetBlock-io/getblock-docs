---
description: >-
  Example code for the eth_syncing JSON RPC method. Сomplete guide on how to use
  eth_syncing JSON RPC in GetBlock Web3 documentation.
---

# eth\_syncing - BSC

This method returns information about the sync status of the node. When syncing, it returns an object with sync progress. When fully synced, it returns `false`.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_syncing",
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
    method: 'eth_syncing',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const result = response.data.result;
    if (result === false) {
        console.log('Node is fully synced');
    } else {
        console.log('Syncing:', result);
    }
})
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
result = response.json()["result"]
if result == False:
    print("Node is fully synced")
else:
    print(f"Syncing: {result}")
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
        "method": "eth_syncing",
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
    "result": false
}
```

## Response Parameters

| Parameter | Type           | Description                        |
| --------- | -------------- | ---------------------------------- |
| result    | boolean/object | false if synced, object if syncing |

| Field         | Type   | Description         |
| ------------- | ------ | ------------------- |
| startingBlock | string | Block sync started  |
| currentBlock  | string | Current block       |
| highestBlock  | string | Highest known block |

## Use Cases

* Check node health before queries
* Monitor node synchronization
* Verify RPC endpoint status
* Build node monitoring dashboards

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncStatus = await provider.send('eth_syncing', []);
console.log('Syncing:', syncStatus);
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

const syncStatus = await client.request({ method: 'eth_syncing' });
console.log('Syncing:', syncStatus);
```
{% endtab %}
{% endtabs %}
