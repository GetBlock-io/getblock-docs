---
description: >-
  Example code for the eth_accounts JSON RPC method. Сomplete guide on how to
  use eth_accounts JSON RPC in GetBlock Web3 documentation.
---

# eth\_accounts - BSC

This method returns a list of addresses owned by the client. For public RPC endpoints like GetBlock, this typically returns an empty array since no private keys are stored server-side. This method is primarily useful when connecting to a local node with unlocked accounts.

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
    "method": "eth_accounts",
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
    method: 'eth_accounts',
    params: [],
    id: 'getblock.io'
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
    "method": "eth_accounts",
    "params": [],
    "id": "getblock.io"
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
        "method": "eth_accounts",
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
    "result": []
}
```

## Response Parameters

| Parameter | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| jsonrpc   | string | JSON-RPC version (2.0)               |
| id        | string | Request identifier                   |
| result    | array  | Empty array for public RPC endpoints |

## Use Cases

* Check for locally stored accounts on a node
* Verify RPC endpoint configuration
* Wallet integration testing
* Node account management verification

## Error Handling

| Error Code | Description                               |
| ---------- | ----------------------------------------- |
| -32601     | Method not found - method may be disabled |
| -32603     | Internal error - node processing issues   |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Note: This returns empty array for public RPC
const accounts = await provider.send('eth_accounts', []);
console.log(accounts);
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

// Note: Returns empty for public RPC
const accounts = await client.request({ method: 'eth_accounts' });
console.log(accounts);
```
{% endtab %}
{% endtabs %}
