---
description: >-
  Example code for the net_peerCount json-rpc method. Сomplete guide on how to
  use net_peerCount json-rpc in GetBlock.io Web3 documentation.
---

# net\_peerCount - Optimism

This method returns the number of peers the node is connected to in the P2P network. A higher peer count generally indicates better network connectivity and reliability. This is useful for monitoring node health.

### Parameters

* None

### Request Sample

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "net_peerCount",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'net_peerCount',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const peerCount = parseInt(response.data.result, 16);
    console.log('Peer Count:', peerCount);
})
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="Python" overflow="wrap" %}
```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "net_peerCount",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
peer_count = int(response.json()["result"], 16)
print(f"Peer Count: {peer_count}")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "net_peerCount",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x12d"
}
```

### Response Parameters

* **`id`**: A unique request identifier, matching the `id` sent in the request body.
* **`jsonrpc`**: Specifies the use of JSON-RPC version 2.0.
* **`result`**: The number of connected peers as a hexadecimal string. `0x19` equals 25 peers.

### Use Case

The net\_peerCount method is commonly used for:

* **Network connectivity monitoring**
* **Node health assessment**
* **P2P network analysis**

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const peerCount = await provider.send('net_peerCount', []);
console.log('Peer Count:', parseInt(peerCount, 16));
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

const client = createPublicClient({
    chain: optimism,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const peerCount = await client.request({ method: 'net_peerCount' });
console.log('Peer Count:', parseInt(peerCount, 16));
```
{% endcode %}
{% endtab %}
{% endtabs %}
