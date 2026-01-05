---
description: >-
  Example code for the eth_getProof JSON-RPC method. Complete guide on how to
  use eth_getProof JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getProof - Mantle

This method returns the account and storage values of the specified account including the Merkle proof. This is useful for verifying state without trusting the RPC provider.

### Parameters

| Parameter   | Type   | Description                    |
| ----------- | ------ | ------------------------------ |
| address     | string | The address of the account     |
| storageKeys | array  | Array of storage keys to prove |
| blockNumber | string | Block number (hex) or tag      |

### Request

{% tabs %}
{% tab title="curl" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        ["0x0"],
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (axios)" %}
{% code title="example.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        ["0x0"],
        "latest"
    ],
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

{% tab title="Python (requests)" %}
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        ["0x0"],
        "latest"
    ],
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

{% tab title="Rust (reqwest)" %}
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
            "method": "eth_getProof",
            "params": [
                "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
                ["0x0"],
                "latest"
            ],
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

{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const proof = await provider.send('eth_getProof', [
    '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
    ['0x0'],
    'latest'
]);
console.log('Proof:', proof);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mantle } from 'viem/chains';

const client = createPublicClient({
    chain: mantle,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const proof = await client.getProof({
    address: '0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE',
    storageKeys: ['0x0']
});
console.log('Proof:', proof);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "address": "0x201eba5cc46d216ce6dc03f6a759e8e766e956ae",
        "accountProof": ["0xf90211a0..."],
        "balance": "0x0",
        "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "nonce": "0x0",
        "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "storageProof": []
    }
}
```

### Response Parameters

| Field        | Type   | Description                                |
| ------------ | ------ | ------------------------------------------ |
| address      | string | The address of the account                 |
| accountProof | array  | Array of RLP-serialized Merkle proof nodes |
| balance      | string | Account balance in wei (hex)               |
| codeHash     | string | Hash of the account code                   |
| nonce        | string | Account nonce (hex)                        |
| storageHash  | string | Storage root hash                          |
| storageProof | array  | Array of storage proofs                    |

### Use Case

The `eth_getProof` method is essential for:

* Light client verification
* Trustless state validation
* Cross-chain bridges
* State proof verification
* Decentralized applications requiring proof

### Error Handling

| Status Code | Error Message  | Cause                           |
| ----------- | -------------- | ------------------------------- |
| 403         | Forbidden      | Missing or invalid ACCESS-TOKEN |
| -32602      | Invalid params | Invalid address or storage keys |

### Web3 Integration

{% tabs %}
{% tab title="Ether.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function fetchBlockNumberFromProvider() {
  try {
    // Call the method — this will resolve to a number (promise resolves to number)
     const result = await provider.send("eth_getProof", [
        "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        ["0x0"],
        "latest"
    ],);
    console.log("Block number result:", result);
    return result;
  } catch (error) {
    console.error("Ethers Error fetching block number:", error);
    throw error;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
```jsx
import { createPublicClient, http } from "viem";
import { hyperEvm } from "viem/chains";

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: hyperEvm,
  transport: http(import.meta.env.MANTLE_ACCESS_TOKEN),
});

// Using the method through Viem
async function Call() {
  try {
    // Method-specific Viem implementation
    const result = await client.request({
      method: "eth_getProof",
      params: [
        "0x201EBa5CC46D216Ce6DC03F6a759e8E766e956aE",
        ["0x0"],
        "latest"
    ],
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
