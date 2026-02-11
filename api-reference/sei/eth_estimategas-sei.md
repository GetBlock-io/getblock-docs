---
description: >-
  Example code for the eth_estimateGas JSON-RPC method. Complete guide on how to
  use eth_estimateGas JSON-RPC in GetBlock Web3 documentation.
---

# eth\_estimateGas - SEI

Estimates the gas necessary to complete a transaction on the Sei network. The estimate may be higher than the actual gas used.

## Parameters

| Parameter   | Type   | Description                                                                         |
| ----------- | ------ | ----------------------------------------------------------------------------------- |
| transaction | object | The transaction object with 'from', 'to', 'gas', 'gasPrice', 'value', 'data' fields |
| block       | string | Block number in hex, or 'latest', 'earliest', 'pending' (optional)                  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (axios)" %}
```javascript
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
    jsonrpc: '2.0',
    method: 'eth_estimateGas',
    params: [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}],
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

{% tab title="Request" %}
{% code title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_estimateGas",
    "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.json())
```
{% endcode %}


{% endtab %}

{% tab title="Rust" %}
{% code title="Rust (reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_estimateGas",
            "params": [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}],
            "id": "getblock.io"
        }))
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

## Response

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

| Field  | Type   | Description                                      |
| ------ | ------ | ------------------------------------------------ |
| result | string | The estimated gas amount as a hexadecimal string |

## Use Case

The `eth_estimateGas` method is essential for:

* Blockchain developers building applications on Sei
* Wallet applications requiring network data
* Analytics platforms tracking Sei network activity
* DeFi protocols integrating with Sei's parallelized EVM

## Error Handling

Common errors when using this method:

| Error Code | Message          | Description                                |
| ---------- | ---------------- | ------------------------------------------ |
| -32700     | Parse error      | Invalid JSON                               |
| -32600     | Invalid Request  | JSON is not a valid request object         |
| -32601     | Method not found | Method does not exist                      |
| -32602     | Invalid params   | Invalid method parameters                  |
| -32603     | Internal error   | Internal JSON-RPC error                    |
| -32000     | Invalid input    | Generic input error                        |
| -32500     | Cross-VM error   | Error in cross-VM operation (Sei-specific) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Using ethers provider methods
const result = await provider.send('eth_estimateGas', [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}]);
console.log(result);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sei } from 'viem/chains';

const client = createPublicClient({
    chain: sei,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Using viem's request method
const result = await client.request({
    method: 'eth_estimateGas',
    params: [{"from": "0x742d35Cc6634C0532925a3b844Bc9e7595f5bE21", "to": "0x1234567890abcdef1234567890abcdef12345678", "value": "0x9184e72a"}]
});
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}
