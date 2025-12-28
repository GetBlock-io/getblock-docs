---
description: >-
  Example code for the eth_getStorageAt JSON-RPC method. Complete guide on how
  to use eth_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Monad

This method returns the value from a storage position at a given address.

## Parameters

| Parameter   | Type   | Required | Description                                                                   |
| ----------- | ------ | -------- | ----------------------------------------------------------------------------- |
| address     | string | Yes      | The address of the storage.                                                   |
| position    | string | Yes      | The storage position (hex).                                                   |
| blockNumber | string | No       | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized". |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0xdAC17F958D2ee523a2206206994597C13D831ec7", "0x0", "latest"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": ["0xdAC17F958D2ee523a2206206994597C13D831ec7", "0x0", "latest"],
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
    "method": "eth_getStorageAt",
    "params": ["0xdAC17F958D2ee523a2206206994597C13D831ec7", "0x0", "latest"],
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
{% code title="example.rs" %}
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
            "params": ["0xdAC17F958D2ee523a2206206994597C13D831ec7", "0x0", "latest"],
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
    "result": "0x000000000000000000000000000000000000000000000000000000000000000a"
}
```

## Response Parameters

| Field  | Type   | Description                                                |
| ------ | ------ | ---------------------------------------------------------- |
| result | string | The value at the storage position (32 bytes, hex encoded). |

## Calculating Storage Slots

For mappings and dynamic arrays, use keccak256 to calculate the slot:

{% code title="calculate-slot.js" %}
```javascript
import { ethers } from 'ethers';

// For mapping: mapping(address => uint256)
// Storage slot = keccak256(abi.encode(key, mappingSlot))
const key = '0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb';
const mappingSlot = 0; // First state variable

const slot = ethers.keccak256(
    ethers.AbiCoder.defaultAbiCoder().encode(
        ['address', 'uint256'],
        [key, mappingSlot]
    )
);
```
{% endcode %}

## Use Case

The `eth_getStorageAt` method is essential for:

* Reading contract state directly
* Analyzing contract storage layout
* Security auditing
* Verifying contract state without ABI
* Debugging contract behavior
* Historical state analysis

## Error Handling

| Status Code | Error Message      | Cause                               |
| ----------- | ------------------ | ----------------------------------- |
| 403         | Forbidden          | Missing or invalid ACCESS-TOKEN.    |
| -32602      | Invalid params     | Invalid address or position format. |
| -32000      | Resource not found | Block not found.                    |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Read storage slot 0
const value = await provider.getStorage(contractAddress, 0);
console.log('Slot 0:', value);

// Read specific slot
const slot = '0x0000000000000000000000000000000000000000000000000000000000000001';
const value1 = await provider.getStorage(contractAddress, slot);
console.log('Slot 1:', value1);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="web3.js" %}
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
      method: "eth_getStorageAt",
      params: ["0xdAC17F958D2ee523a2206206994597C13D831ec7", "0x0", "latest"],
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
