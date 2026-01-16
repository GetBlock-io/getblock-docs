---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Somnia

This method returns the value from a storage position at a given address on the Somnia network. This is useful for reading raw contract storage, analyzing state, and debugging smart contracts.

## Parameters

| Parameter   | Type   | Required | Description                      |
| ----------- | ------ | -------- | -------------------------------- |
| address     | string | Yes      | Contract address                 |
| position    | string | Yes      | Storage slot position (hex)      |
| blockNumber | string | Yes      | Block number in hex, or "latest" |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getStorageAt",
  "params": [
    "0xContractAddress",
    "0x0",
    "latest"
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getStorageAt',
  params: ['0xContractAddress', '0x0', 'latest']
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
{% code title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getStorageAt",
    "params": ["0xContractAddress", "0x0", "latest"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
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
        "id": "getblock.io",
        "method": "eth_getStorageAt",
        "params": ["0xContractAddress", "0x0", "latest"]
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
  "result": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```

## Response Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| result    | string | 32-byte storage value at position |

## Use Cases

* Read contract storage directly
* Debug storage layout
* Verify state variables
* Analyze proxy contracts
* Security auditing

## Error Handling

| Error Code | Description                                    |
| ---------- | ---------------------------------------------- |
| -32602     | Invalid params - malformed address or position |
| -32603     | Internal error - node processing issues        |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const storage = await provider.getStorage('0xContractAddress', 0);
console.log(storage);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const storage = await client.getStorageAt({
  address: '0xContractAddress',
  slot: '0x0'
});
console.log(storage);
```
{% endcode %}
{% endtab %}
{% endtabs %}
