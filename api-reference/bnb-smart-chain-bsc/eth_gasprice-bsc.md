---
description: >-
  Example code for the eth_gasPrice JSON RPC method. Сomplete guide on how to
  use eth_gasPrice JSON RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - BSC

This method returns the current gas price on the BNB Smart Chain in wei. BSC is known for its low transaction fees, typically around $0.10 or less per transaction. This method is essential for calculating transaction costs and setting appropriate gas prices.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
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
    method: 'eth_gasPrice',
    params: [],
    id: 'getblock.io'
};

axios.post(url, payload, {
    headers: { 'Content-Type': 'application/json' }
})
.then(response => {
    const gasPriceWei = parseInt(response.data.result, 16);
    const gasPriceGwei = gasPriceWei / 1e9;
    console.log('Gas Price:', gasPriceGwei, 'Gwei');
})
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
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
gas_price_wei = int(response.json()["result"], 16)
gas_price_gwei = gas_price_wei / 1e9
print(f"Gas Price: {gas_price_gwei} Gwei")
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
        "method": "eth_gasPrice",
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
    "result": "0x12a05f200"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC version (2.0)                  |
| id        | string | Request identifier                      |
| result    | string | Gas price in wei (0x12a05f200 = 5 Gwei) |

## Use Cases

* Estimate transaction costs on BSC
* Set appropriate gas prices for transactions
* Monitor network congestion
* Calculate fee budgets for DeFi applications
* Compare costs across different networks

## Error Handling

| Error Code | Description                             |
| ---------- | --------------------------------------- |
| -32603     | Internal error - node processing issues |
| -32000     | Server error - RPC endpoint unavailable |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const feeData = await provider.getFeeData();
console.log('Gas Price:', ethers.formatUnits(feeData.gasPrice, 'gwei'), 'Gwei');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http, formatGwei } from 'viem';
import { bsc } from 'viem/chains';

const client = createPublicClient({
    chain: bsc,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasPrice = await client.getGasPrice();
console.log('Gas Price:', formatGwei(gasPrice), 'Gwei');
```
{% endcode %}
{% endtab %}
{% endtabs %}
