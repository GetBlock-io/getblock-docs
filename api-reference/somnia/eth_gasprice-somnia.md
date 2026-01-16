---
description: >-
  Example code for the eth_gasPrice JSON-RPC method. Complete guide on how to
  use eth_gasPrice JSON-RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - Somnia

This method returns the current gas price on the Somnia network in wei. Somnia features dynamic gas pricing with sub-cent transaction fees, making it highly economical for high-throughput applications. This method is essential for estimating transaction costs and setting appropriate gas prices.

## Parameters

* None

## Returns

| Field  | Type   | Description                             |
| ------ | ------ | --------------------------------------- |
| result | string | Current gas price in wei as hexadecimal |

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
  "method": "eth_gasPrice",
  "params": []
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
  id: 'getblock.io',
  method: 'eth_gasPrice',
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

{% tab title="Request" %}
{% code title="Python" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_gasPrice",
    "params": []
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
        "method": "eth_gasPrice",
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

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x3b9aca00"
}
```

## Response Parameters

| Parameter | Type   | Description                            |
| --------- | ------ | -------------------------------------- |
| result    | string | Gas price in wei (0x3b9aca00 = 1 Gwei) |

## Use Cases

* Estimate transaction costs
* Set appropriate gas prices for transactions
* Monitor network congestion
* Calculate fee budgets for dApps
* Implement gas price strategies

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="First Tab" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasPrice = await provider.getFeeData();
console.log(ethers.formatUnits(gasPrice.gasPrice, 'gwei')); // Convert to Gwei
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasPrice = await client.getGasPrice();
console.log(formatGwei(gasPrice));
```
{% endcode %}
{% endtab %}
{% endtabs %}
