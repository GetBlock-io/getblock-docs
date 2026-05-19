---
description: >-
  Example code for the eth_gasPrice JSON-RPC method. Complete guide on how to
  use eth_gasPrice JSON-RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - ARC

This method returns the node's current suggested gas price, denominated in **USDC base units** (not wei). Multiply by `gasUsed` to get the total USDC cost of a transaction. Arc's dollar-denominated fees make cost forecasting predictable in ways that volatile gas-token chains cannot match.

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
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
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
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    const client = Client::new();

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
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3b9aca00"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                             |
| ------ | ------ | --------------------------------------------------------------------------------------- |
| result | string | Current suggested gas price in USDC base units (hex). To convert to USDC, divide by 10⁶ |

## Use Cases

* Setting `gasPrice` on legacy transactions
* Fallback for clients that don't support EIP-1559
* Forecasting USDC payment costs before broadcast
* Merchant invoicing systems that need predictable fee budgets

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_gasPrice', []);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    network: 'arc-testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } },
    blockExplorers: { default: { name: 'arcscan', url: 'https://testnet.arcscan.app' } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_gasPrice', params: [] });
console.log(result);
```
{% endtab %}
{% endtabs %}
