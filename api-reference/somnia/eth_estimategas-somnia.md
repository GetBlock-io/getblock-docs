---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - Somnia

This method generates an estimate of how much gas is necessary to complete a transaction on the Somnia network. This is essential for setting appropriate gas limits. Somnia's IceDB provides deterministic performance, enabling more accurate gas estimation than traditional blockchains.

## Parameters

| Parameter   | Type   | Required | Description                                |
| ----------- | ------ | -------- | ------------------------------------------ |
| transaction | object | Yes      | Transaction call object                    |
| blockNumber | string | No       | Block number in hex, or "latest" (default) |

### Transaction Object

| Field    | Type   | Required | Description       |
| -------- | ------ | -------- | ----------------- |
| from     | string | No       | Sender address    |
| to       | string | No       | Recipient address |
| gas      | string | No       | Gas limit         |
| gasPrice | string | No       | Gas price         |
| value    | string | No       | Value to send     |
| data     | string | No       | Transaction data  |

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
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
      "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
      "value": "0xde0b6b3a7640000"
    }
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
  method: 'eth_estimateGas',
  params: [
    {
      from: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
      to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
      value: '0xde0b6b3a7640000'
    }
  ]
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
    "method": "eth_estimateGas",
    "params": [
        {
            "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
            "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
            "value": "0xde0b6b3a7640000"
        }
    ]
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
        "method": "eth_estimateGas",
        "params": [{
            "from": "0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45",
            "to": "0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199",
            "value": "0xde0b6b3a7640000"
        }]
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

{% code title="Response (JSON)" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x5208"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                              |
| --------- | ------ | -------------------------------------------------------- |
| result    | string | Gas estimate in hex (0x5208 = 21000 for simple transfer) |

## Use Cases

* Calculate transaction costs before sending
* Set appropriate gas limits
* Budget gas for complex contract interactions
* Optimize transaction parameters
* Prevent out-of-gas errors

## Error Handling

| Error Code | Description                                    |
| ---------- | ---------------------------------------------- |
| -32602     | Invalid params - malformed transaction object  |
| -32603     | Internal error - execution reverted            |
| -32000     | Execution error - insufficient funds or revert |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const gasEstimate = await provider.estimateGas({
  from: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: ethers.parseEther('1.0')
});
console.log(gasEstimate.toString());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http, parseEther } from 'viem';
import { somnia } from 'viem/chains';

const client = createPublicClient({
  chain: somnia,
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const gasEstimate = await client.estimateGas({
  account: '0x742d35Cc6634C0532925a3b844Bc9e7595f8bB45',
  to: '0x8626f6940E2eb28930eFb4CeF49B2d1F2C9C1199',
  value: parseEther('1')
});
console.log(gasEstimate);
```
{% endcode %}
{% endtab %}
{% endtabs %}
