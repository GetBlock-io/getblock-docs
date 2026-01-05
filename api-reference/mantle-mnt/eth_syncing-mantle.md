---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide on how to use
  eth_syncing JSON-RPC in GetBlock Web3 documentation.
---

# eth\_syncing - Mantle

This method returns an object with data about the sync status or `false` if the Mantle node is fully synced.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
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

{% tab title="JavaScript (axios)" %}
{% code title="eth_syncing.js" %}
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

{% tab title="Python (requests)" %}
{% code title="eth_syncing.py" %}
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

{% tab title="Rust (reqwest)" %}
{% code title="eth_syncing.rs" %}
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
{% code title="response-not-syncing.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": false
}
```
{% endcode %}
{% endtab %}

{% tab title="Syncing" %}
{% code title="response-syncing.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "startingBlock": "0x384",
        "currentBlock": "0x386",
        "highestBlock": "0x454"
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Parameters

| Field         | Type           | Description                            |
| ------------- | -------------- | -------------------------------------- |
| result        | boolean/object | false if synced, or sync status object |
| startingBlock | string         | Block at which sync started            |
| currentBlock  | string         | Current block being synced             |
| highestBlock  | string         | Highest known block                    |

### Use Case

The `eth_syncing` method is essential for:

* Node health monitoring
* Sync progress tracking
* Application readiness checks
* Infrastructure monitoring
* Data reliability verification
* Node deployment validation

### Error Handling

| Status Code | Error Message | Cause                           |
| ----------- | ------------- | ------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN |

### Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.eth_syncing.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const syncing = await provider.send('eth_syncing', []);
console.log('Syncing:', syncing);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.eth_syncing.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const syncing = await client.request({
    method: 'eth_syncing',
    params: []
});
console.log('Syncing:', syncing);
```
{% endcode %}
{% endtab %}
{% endtabs %}
