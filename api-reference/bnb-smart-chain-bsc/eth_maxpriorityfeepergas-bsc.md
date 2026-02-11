---
description: >-
  Example code for the eth_maxPriorityFeePerGas JSON RPC method. Сomplete guide
  on how to use eth_maxPriorityFeePerGas JSON RPC in GetBlock Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - BSC

This method returns the current max priority fee per gas for EIP-1559 transactions. Note that BSC uses legacy gas pricing, so this may return 0 or not be fully implemented.

{% hint style="info" %}
BSC uses legacy gas pricing. This method may return 0 or not be fully implemented on BSC.
{% endhint %}

## Parameters

* none

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
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
    method: 'eth_maxPriorityFeePerGas',
    params: [],
    id: 'getblock.io'
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
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
        "method": "eth_maxPriorityFeePerGas",
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

## Response Example

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0"
}
```

## Response Parameters

| Parameter | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| result    | string | Priority fee (often 0x0 on BSC) |

## Use Cases

* EIP-1559 compatibility checks
* Cross-chain fee estimation

## Error Handling

| Error Code | Description          |
| ---------- | -------------------- |
| -32601     | Method not supported |
| -32603     | Internal error       |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// BSC uses legacy gas - use eth_gasPrice instead
const gasPrice = await provider.getGasPrice();
console.log('Gas Price:', ethers.formatUnits(gasPrice, 'gwei'), 'Gwei');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// BSC uses legacy gas pricing
const gasPrice = await client.getGasPrice();
console.log('Gas Price:', gasPrice);
```
{% endcode %}
{% endtab %}
{% endtabs %}
