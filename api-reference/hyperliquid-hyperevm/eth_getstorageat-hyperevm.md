---
description: >-
  Example code for the eth_getStorageAt JSON RPC method. Сomplete guide on how
  to use eth_getStorageAt GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - HyperEVM

This method returns the value from a storage position at a given address.

## Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| address   | string | Yes      | Address of the contract.                   |
| position  | string | Yes      | Storage position (hex).                    |
| block     | string | Yes      | Block parameter (only "latest" supported). |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0xContractAddress", "0x0", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0xContractAddress", "0x0", "latest"],
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
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0xContractAddress", "0x0", "latest"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
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
            "method": "eth_getStorageAt",
            "params": ["0xContractAddress", "0x0", "latest"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
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
    "result": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                    |
| ------ | ------ | ---------------------------------------------- |
| result | string | Storage value at the position (32 bytes, hex). |

## Use Case

The `eth_getStorageAt` method is essential for:

* Reading contract storage directly
* Debugging smart contracts
* Analyzing contract state
* Security auditing
* Reverse engineering contract data

## Error Handling

| Error Code | Message        | Cause                        |
| ---------- | -------------- | ---------------------------- |
| -32602     | Invalid params | Invalid address or position. |
| -32603     | Internal error | Node issue.                  |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

async function getStorage() {
    const value = await provider.getStorage('0xContractAddress', 0);
    console.log('Storage Value:', value);
}

getStorage();
```
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.HYPER_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
      method: "eth_getStorageAt",
      params: ["0xContractAddress", "0x0", "latest"],
    });
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endtab %}
{% endtabs %}

