---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide on how to use
  eth_syncing JSON-RPC in GetBlock Web3 documentation.
---

# eth\_syncing - Monad

This method returns an object with data about the sync status or `false` if not syncing.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
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
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="request.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="request.rs" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "eth_syncing",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% tabs %}
{% tab title="Not Syncing" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```
{% endtab %}

{% tab title="Syncing" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "startingBlock": "0x0",
        "currentBlock": "0x1b4",
        "highestBlock": "0x1c0"
    }
}
```
{% endtab %}
{% endtabs %}

### Response Parameters

| Field         | Type           | Description                                  |
| ------------- | -------------- | -------------------------------------------- |
| result        | boolean/object | False if not syncing, or sync status object. |
| startingBlock | string         | Block number where sync started (hex).       |
| currentBlock  | string         | Current block being synced (hex).            |
| highestBlock  | string         | Highest known block (hex).                   |

### Use Case

The `eth_syncing` method is essential for:

* Node health monitoring
* Sync progress tracking
* Application readiness checks
* Infrastructure monitoring
* Load balancer health checks
* User notifications about node status

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncStatus = await provider.send('eth_syncing', []);

if (syncStatus === false) {
    console.log('Node is fully synced');
} else {
    const current = parseInt(syncStatus.currentBlock, 16);
    const highest = parseInt(syncStatus.highestBlock, 16);
    const progress = ((current / highest) * 100).toFixed(2);
    console.log(`Syncing: ${progress}% (${current}/${highest})`);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

