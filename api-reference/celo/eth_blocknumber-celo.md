---
description: >-
  Example code for the eth_blockNumber JSON-RPC method. Complete guide on how to
  use eth_blockNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_blockNumber - Celo

This method returns the number of the most recent block on the Celo network. This is a fundamental method for tracking blockchain state, monitoring network progress, and determining transaction finality. With Celo's \~1 second block times, this value updates rapidly.

### Parameters

* None

### Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_blockNumber",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_blockNumber',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_blockNumber",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "eth_blockNumber",
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
{% endcode %}
{% endtab %}
{% endtabs %}

### Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x1d9f2a8"
}
```

### Response Parameters Definition

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| result    | string | Hexadecimal block number (e.g., "0x1d9f2a8" = 31,130,280) |

### Use Cases

* Monitor blockchain synchronization status
* Calculate transaction confirmations
* Track network liveness and progress
* Implement block-based polling for updates
* Determine finality for transactions

### Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

### SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await provider.getBlockNumber();
console.log(blockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { celo } from 'viem/chains';

const client = createPublicClient({
  chain: celo,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const blockNumber = await client.getBlockNumber();
console.log(blockNumber);
```
{% endcode %}
{% endtab %}

{% tab title="ContractKit" %}
{% code title="contractkit.js" %}
```javascript
const { newKit } = require('@celo/contractkit');

const kit = newKit('https://go.getblock.io/<ACCESS-TOKEN>/');

const blockNumber = await kit.web3.eth.getBlockNumber();
console.log(blockNumber);
```
{% endcode %}
{% endtab %}
{% endtabs %}
