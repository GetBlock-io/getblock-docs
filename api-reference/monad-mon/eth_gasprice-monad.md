---
description: >-
  Example code for the eth_gasPrice JSON-RPC method. Complete guide on how to
  use eth_gasPrice JSON-RPC in GetBlock Web3 documentation.
---

# eth\_gasPrice - Monad

This method returns the current price per gas in MON-wei.

## Parameters

* None

## Request

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

{% tab title="Axios" %}
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
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Request" %}
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

{% tab title="Rust" %}
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
            "method": "eth_gasPrice",
            "params": [],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3b9aca00"
}
```

## Response Parameters

| Field  | Type   | Description                                                      |
| ------ | ------ | ---------------------------------------------------------------- |
| result | string | Current gas price in MON-wei (hexadecimal). 0x3b9aca00 = 1 Gwei. |

## Use Case

The `eth_gasPrice` method is essential for:

* Setting appropriate gas prices for transactions
* Cost estimation for users
* Fee calculation for wallets
* Gas price monitoring dashboards
* Automated trading gas optimization
* DeFi transaction planning

## Error Handling

| Status Code | Error Message  | Cause                            |
| ----------- | -------------- | -------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN. |
| -32603      | Internal error | Node error retrieving gas price. |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasPrice = await provider.getGasPrice();
console.log('Gas price:', ethers.formatUnits(gasPrice, 'gwei'), 'Gwei');

// For EIP-1559 transactions
const feeData = await provider.getFeeData();
console.log('Max fee:', ethers.formatUnits(feeData.maxFeePerGas, 'gwei'), 'Gwei');
console.log('Priority fee:', ethers.formatUnits(feeData.maxPriorityFeePerGas, 'gwei'), 'Gwei');
```
{% endtab %}

{% tab title="Web3.js" %}
```javascript
import { createPublicClient, http } from "viem";
import { monad } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: monad,
  transport: http(
    import.meta.env.MONAD_ACCESS_TOKEN
  ),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
     method: "eth_gasPrice",
     params: [],
    });
    console.log("Result:", result);
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}
