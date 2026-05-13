---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Optimism

This method retrieves a specific transaction from a block by combining the block's hash with the transaction's index position within that block. The index is zero-based, so the first transaction has index 0. This is useful when you need to access transactions in order within a block or iterate through all transactions in a specific block.

## Parameters

| Parameter        | Type           | Description                                    | Required |
| ---------------- | -------------- | ---------------------------------------------- | -------- |
| blockHash        | DATA, 32 Bytes | The hash of a block.                           | Yes      |
| transactionIndex | QUANTITY       | The transaction index position in hexadecimal. | Yes      |

## Request Sample

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
       "method": "eth_getTransactionByBlockHashAndIndex",
 "params": [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
 "params": [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
    ],
    "id": "getblock.io"
};

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://go.getblock.us/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
  .then((response) => {
    console.log(JSON.stringify(response.data));
  })
  .catch((error) => {
    console.log(error);
  });

```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.us/<ACESS_TOKEN>"

payload = json.dumps({
   "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
 "params": [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
    ],
    "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" %}
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
   "jsonrpc": "2.0",
       "method": "eth_getTransactionByBlockHashAndIndex",
 "params": [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
    ],
    "id": "getblock.io"
}"#;

    let json: serde_json::Value = serde_json::from_str(&data)?;

    let request = client.request(reqwest::Method::POST, "https://go.getblock.us/<ACCESS_TOKEN>")
        .headers(headers)
        .json(&json);

    let response = request.send().await?;
    let body = response.text().await?;

    println!("{}", body);

    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

A successful response returns the following:

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
        "blockNumber": "0x9084e08",
        "blockTimestamp": "0x6a0475c9",
        "from": "0xdeaddeaddeaddeaddeaddeaddeaddeaddead0001",
        "gas": "0xf4240",
        "gasPrice": "0x0",
        "hash": "0x8298e19f39ad1e9a2d670739614fafbbed1d5b3824319384f479168d2d8c8f95",
        "input": "0x3db6be2b0000146b000f79c50000000000000003000000006a04756f00000000017ec99d0000000000000000000000000000000000000000000000000000000020017d620000000000000000000000000000000000000000000000000000000001d918f3af6d6abe101cdf792f420a962b62de02ba52726b1d21f5b9d43aff3c572b27cf0000000000000000000000006887246668a3b87f54deb3b94ba47a6f63f329850000000000000000000000000190",
        "nonce": "0x2c28b94",
        "to": "0x4200000000000000000000000000000000000015",
        "transactionIndex": "0x0",
        "value": "0x0",
        "type": "0x7e",
        "v": "0x0",
        "r": "0x0",
        "s": "0x0",
        "sourceHash": "0xc912db3c7247e182b351c44e56b691ebcafbc8ada96c394ee5f5aa65d90b930e",
        "mint": "0x0",
        "depositReceiptVersion": "0x1"
    }
}

```
{% endcode %}

## Response Parameters

| Field            | Type           | Description                                     |
| ---------------- | -------------- | ----------------------------------------------- |
| hash             | string         | Transaction hash                                |
| blockHash        | string         | Hash of the block this transaction belongs to   |
| blockNumber      | string (hex)   | Block number                                    |
| from             | string         | Address that initiated the transaction          |
| to               | string or null | Recipient address or null for contract creation |
| value            | string (hex)   | Amount transferred in wei                       |
| nonce            | string (hex)   | Number of transactions sent by the sender       |
| gas              | string (hex)   | Gas limit                                       |
| gasPrice         | string (hex)   | Gas price in wei                                |
| input            | string (hex)   | Transaction calldata                            |
| transactionIndex | string (hex)   | Index of this transaction in the block          |
| v, r, s          | string         | Signature fields                                |

## Use Case

#### Error handling

| Status Code | Error Message    | Cause                                                                                                                                       |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                                                                           |
| -32602      | Invalid argument | <ul><li>The block hash does not exist</li><li>Index is not a proper hex integer</li><li>Block does have transaction at that index</li></ul> |

#### Integration with Web3

The `eth_getTransactionByBlockHashAndIndex` method helps developers to:

* Build richer block explorers that display ordered transaction lists
* Generate analytics around transaction distribution inside blocks
* Debug transactions by locating their exact place in the block
* Power DeFi dashboards and monitoring tools that react to specific activity
* Build trustless frontends that fetch live block data without running a node

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send("eth_getTransactionByBlockHashAndIndex",
     [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
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
        method: "eth_getTransactionByBlockHashAndIndex",
 params: [
"0x90caa11f7bec1b9836af2ff235ccce608fbd826b6c3dbc7928ffe94869c59da2",
"0x0"
    ]
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
