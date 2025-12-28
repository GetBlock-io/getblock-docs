---
description: >-
  Example code for the eth_call JSON RPC method. Сomplete guide on how to use
  eth_call GetBlock JSON RPC in GetBlock Web3 documentation.
---

# eth\_call - HyperEVM

This method executes a new message call immediately without creating a transaction on the blockchain.

## Parameters

| Parameter   | Type   | Required | Description                                |
| ----------- | ------ | -------- | ------------------------------------------ |
| transaction | object | Yes      | Transaction call object.                   |
| block       | string | Yes      | Block parameter (only "latest" supported). |

### Transaction Object

| Field    | Type   | Required | Description                 |
| -------- | ------ | -------- | --------------------------- |
| from     | string | No       | Sender address.             |
| to       | string | Yes      | Contract address to call.   |
| gas      | string | No       | Gas limit (hex).            |
| gasPrice | string | No       | Gas price (hex).            |
| value    | string | No       | Value to send (hex).        |
| data     | string | No       | Encoded function call data. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0xContractAddress",
        "data": "0x70a08231000000000000000000000000AddressHere"
    }, "latest"],
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
    "method": "eth_call",
    "params": [{
        "to": "0xContractAddress",
        "data": "0x70a08231000000000000000000000000AddressHere"
    }, "latest"],
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

payload = json.dumps(
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
        "to": "0xContractAddress",
        "data": "0x70a08231000000000000000000000000AddressHere"
    }, "latest"],
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
    "method": "eth_call",
    "params": [{
        "to": "0xContractAddress",
        "data": "0x70a08231000000000000000000000000AddressHere"
    }, "latest"],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
}
```

## Response Parameters

| Field  | Type   | Description                               |
| ------ | ------ | ----------------------------------------- |
| result | string | Return data from the contract call (hex). |

## Use Case

The `eth_call` method is essential for:

* Reading smart contract state
* Token balance queries (ERC-20 balanceOf)
* Price feed lookups
* Simulating transactions before sending
* DeFi protocol integrations

## Error Handling

| Error Code | Message            | Cause                       |
| ---------- | ------------------ | --------------------------- |
| -32602     | Invalid params     | Invalid transaction object. |
| -32603     | Internal error     | Contract execution error.   |
| 3          | Execution reverted | Contract reverted the call. |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.io/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);

async function Call() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
    const result = await provider.send("eth_call",
 [{
   "to": "0xContractAddress",
   "data": "0x70a08231000000000000000000000000AddressHere"
 ,
  }]);
    console.log("The result:", result);
    return result;
  } catch (error) {
    console.error("The error:", error);
    throw error;
  }
}
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
       method: "eth_call",
 params: [{
   "to": "0xContractAddress",
   "data": "0x70a08231000000000000000000000000AddressHere"
 ,
]});
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
