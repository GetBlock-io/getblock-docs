---
description: >-
  Example code for the eth_getTransactionCount JSON-RPC method. Complete guide
  on how to use eth_getTransactionCount JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getTransactionCount - Somnia

This method returns the number of transactions sent from an address (the nonce) on the Somnia network. This is critical for transaction construction, as each transaction must have the correct nonce to be valid. With Somnia's high throughput, nonce management is important for rapid transaction submission.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| address     | string | Yes      | The address to get nonce for                            |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getTransactionCount",
  "params": [
    "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
    "latest"
  ]
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
  method: 'eth_getTransactionCount',
  params: ['0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45', 'latest']
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
    "method": "eth_getTransactionCount",
    "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45", "latest"]
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
        "method": "eth_getTransactionCount",
        "params": ["0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45", "latest"]
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

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x15"
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| result    | string | Nonce in hex (0x15 = 21 transactions sent) |

## Use Cases

* Get nonce for transaction signing
* Track account activity
* Detect transaction count gaps
* Manage transaction queues
* Build transaction batching systems

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32602     | Invalid params - malformed address      |
| -32603     | Internal error - node processing issues |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const nonce = await provider.getTransactionCount('0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45');
console.log(nonce);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const nonce = await client.getTransactionCount({
  address: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45'
});
console.log(nonce);
```
{% endcode %}
{% endtab %}
{% endtabs %}
