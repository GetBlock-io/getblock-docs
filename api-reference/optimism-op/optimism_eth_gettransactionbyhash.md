---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Optimism

This method retrieves comprehensive transaction data, including the sender, recipient, value, gas parameters, input data, and block information. It's one of the most commonly used methods for tracking transaction status and retrieving transaction details. The method returns null if the transaction is not found. On Optimism, the transaction object may include additional L2-specific fields.

### Parameters

| Parameter       | Type           | Description                | Required |
| --------------- | -------------- | -------------------------- | -------- |
| transactionHash | DATA, 32 Bytes | The hash of a transaction. | Yes      |

### Request

{% tabs %}
{% tab title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Javascript(axios)" %}
{% code overflow="wrap" %}
```bash
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const headers = { "Content-Type": "application/json" };

const payload = {
    jsonrpc: "2.0",
    method: "eth_getTransactionByHash",
    params: ["0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"],
    id: "getblock.io"
};

axios.post(url, payload, { headers })
    .then(response => {
        if (response.status === 200) {
            console.log("eth_getTransactionByHash result:", response.data.result);
        } else {
            console.error("Error:", response.status, response.statusText);
        }
    })
    .catch(error => {
        console.error("Error:", error.response ? error.response.data : error.message);
    });
```
{% endcode %}
{% endtab %}

{% tab title="Python(request)" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionByHash result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": ["0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

if response.status_code == 200:
    result = response.json().get("result")
    print("eth_getTransactionByHash result:", result)
else:
    print("Error:", response.status_code, response.text)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

A successful response returns the following:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467",
        "blockNumber": "0x9087826",
        "blockTimestamp": "0x6a04ca05",
        "from": "0xcde377407d9b819efc0e185aa2d442194c2233e6",
        "gas": "0x27ac40",
        "gasPrice": "0x15f",
        "maxFeePerGas": "0x5f5e100",
        "maxPriorityFeePerGas": "0x1",
        "hash": "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685",
        "input": "0x83730cc30000000000000000000000000000000000000000000000000000000009087826",
        "nonce": "0xb1ebba",
        "to": "0x4aed348173bbf17dfaeb91598321904c9323d36e",
        "transactionIndex": "0x23",
        "value": "0x0",
        "type": "0x2",
        "accessList": [],
        "chainId": "0xa",
        "v": "0x1",
        "r": "0x4d68c5f11ba8c7803a3abe897b9ba8956e0938bf9338e2467ebc0b10996b85cd",
        "s": "0x5e76ac945fa081a13869c9f3adb9f5484b0b89f074322ec5015ef913b0bb6e0a",
        "yParity": "0x1"
    }
}

```

### Response Parameters

| Field            | Type           | Description                                                       |
| ---------------- | -------------- | ----------------------------------------------------------------- |
| hash             | string         | Hash of the transaction                                           |
| blockHash        | string or null | Hash of the block that contains this transaction. Null if pending |
| blockNumber      | string or null | Block number in hex. Null if pending                              |
| transactionIndex | string or null | Index of this transaction in the block                            |
| from             | string         | Sender address                                                    |
| to               | string or null | Receiver address. Null for contract creation                      |
| value            | string (hex)   | Amount sent in wei                                                |
| nonce            | string (hex)   | Counter of transactions sent by the sender                        |
| gas              | string (hex)   | Gas limit for the transaction                                     |
| gasPrice         | string (hex)   | Gas price in wei                                                  |
| input            | string (hex)   | Calldata or contract code                                         |
| v, r, s          | string         | Transaction signature components                                  |

### Use Case

The eth\_getTransactionByHash method is commonly used for:

* **Transaction tracking**
* **Payment verification**
* **Debugging transactions**
* **Transaction history display**

#### Error handling

| Status Code | Error Message    | Cause                                         |
| ----------- | ---------------- | --------------------------------------------- |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.             |
| -32602      | Invalid argument | <ul><li>Transaction hash is invalid</li></ul> |

#### Integration with Web3

The `eth_getTransactionByHash` method helps developers to:

* Build trustless frontends that check transaction status directly
* Track pending, failed, or confirmed transactions in real time
* Support dashboards, DeFi interfaces, wallets, and explorers
* Read transaction calldata for analytics and debugging
* Improve UX by notifying users instantly when their transaction is included
* Verify transaction ordering for MEV, priority fees, and gas bidding strategies

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getTransactionByHash",
    [
        "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
    ]);    
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
import { createPublicClient, http } from 'viem';
import { optimism } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: optimism,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getTransactionByHash",
         params: [
        "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
    ],
        });
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endtab %}
{% endtabs %}
