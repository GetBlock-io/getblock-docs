---
description: >-
  Example code for the eth_getCode JSON-RPC method. Complete guide on how to use
  eth_getCode JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Monad

This method returns code at a given address. Used to verify if an address is a smart contract.

## Parameters

| Parameter   | Type   | Required | Description                                                                   |
| ----------- | ------ | -------- | ----------------------------------------------------------------------------- |
| address     | string | Yes      | The address to get the code from.                                             |
| blockNumber | string | No       | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized". |

## Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x55962b6c47ae83427d02bcb9832adef4ece7bbf6", "latest"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x55962b6c47ae83427d02bcb9832adef4ece7bbf6", "latest"],
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": ["0x55962b6c47ae83427d02bcb9832adef4ece7bbf6", "latest"],
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
{% code title="main.rs" %}
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
            "method": "eth_getCode",
            "params": ["0x55962b6c47ae83427d02bcb9832adef4ece7bbf6", "latest"],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x606060405260043610610062576000357c0100..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                                     |
| ------ | ------ | ------------------------------------------------------------------------------- |
| result | string | The bytecode at the address. Returns "0x" for EOAs (externally owned accounts). |

## Use Case

The `eth_getCode` method is essential for:

* Verifying if an address is a contract
* Security checks before interactions
* Contract bytecode analysis
* Detecting proxy contracts
* Block explorer functionality
* Smart contract verification

## Error Handling

| Status Code | Error Message      | Cause                            |
| ----------- | ------------------ | -------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN. |
| -32602      | Invalid params     | Invalid address format.          |
| -32000      | Resource not found | Block not found.                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js example" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode("0x55962b6c47ae83427d02bcb9832adef4ece7bbf6");

if (code === '0x') {
    console.log('This is an EOA (externally owned account)');
} else {
    console.log('This is a smart contract');
    console.log('Bytecode length:', (code.length - 2) / 2, 'bytes');
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="" %}
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
      method: "eth_getCode",
      params: ["0x55962b6c47ae83427d02bcb9832adef4ece7bbf6", "latest"],
    });
    console.log("Result:", result);
    return result;
  } catch (error) {
    console.error("Viem Error:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
