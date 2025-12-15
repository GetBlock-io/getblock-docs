---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex JSON RPC
  in GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Arbitrum

This method retrieves a transaction from a block, using the block number and the transaction index within it. This method is essential for inspecting the precise ordering of transactions and analyzing how blocks are structured.

#### Parameters

| Parameter     | Type         | Required | Description                                                                                              |
| ------------- | ------------ | -------- | -------------------------------------------------------------------------------------------------------- |
| block\_number | string       | yes      | The block number encoded as a hex string. Special values: `"latest"`, `"earliest"`, `"pending"`.         |
| index         | string (hex) | yes      | The position of the transaction in the block, encoded as a hex integer (for example `"0x0"` or `"0x5"`). |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getTransactionByBlockNumberAndIndex",
 "params": ["latest", "0x0"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
       "method": "eth_getTransactionByBlockNumberAndIndex",
 "params": ["latest", "0x0"],
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
      "method": "eth_getTransactionByBlockNumberAndIndex",
 "params": ["latest", "0x0"],
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
```rs
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::builder()
        .build()?;

    let mut headers = reqwest::header::HeaderMap::new();
    headers.insert("Content-Type", "application/json".parse()?);

    let data = r#"{
   "jsonrpc": "2.0",
  "method": "eth_getTransactionByBlockNumberAndIndex",
 "params": ["latest", "0x0"],
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0xbb465c64e929a4700a6c5cfc3355e651e0f558134e09e06b4c65657f24499cec",
        "blockNumber": "0x185f29e6",
        "from": "0x00000000000000000000000000000000000a4b05",
        "gas": "0x0",
        "gasPrice": "0x0",
        "hash": "0x61d48ab94f7fccf48f4baa3903781053f932feb466474c037736e910d7ca2afe",
        "input": "0x6bf6a42d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000016ddb2700000000000000000000000000000000000000000000000000000000185f29e60000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0",
        "to": "0x00000000000000000000000000000000000a4b05",
        "transactionIndex": "0x0",
        "value": "0x0",
        "type": "0x6a",
        "chainId": "0xa4b1",
        "v": "0x0",
        "r": "0x0",
        "s": "0x0"
    }
}
```

#### Reponse Parameter Definition

| Field            | Type           | Description                                          |
| ---------------- | -------------- | ---------------------------------------------------- |
| hash             | string         | Hash of the transaction                              |
| blockHash        | string         | Hash of the block this transaction belongs to        |
| blockNumber      | string (hex)   | Block number                                         |
| from             | string         | Sender address                                       |
| to               | string or null | Receiver address or null for contract creation       |
| value            | string (hex)   | Value transferred in wei                             |
| nonce            | string (hex)   | Number of transactions previously sent by the sender |
| gas              | string (hex)   | Gas limit                                            |
| gasPrice         | string (hex)   | Gas price in wei                                     |
| input            | string (hex)   | Transaction calldata                                 |
| transactionIndex | string (hex)   | Index of the transaction inside the block            |
| v, r, s          | string         | ECDSA signature values                               |

#### Use case

The `eth_getTransactionByBlockNumberAndIndex` :

* Show block transaction lists sorted by index
* Helps indexers and analytics platforms reconstruct transaction sequencing
* Useful for debugging or tracking transaction ordering effects like MEV
* Wallets can verify whether a transaction was placed at the expected block position
* Helps monitoring systems detect anomalies such as missing or reordered transactions

#### Error handling

| Status Code | Error Message    | Cause                                                                                                                                                                      |
| ----------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.                                                                                                                                          |
| -32602      | Invalid argument | <ul><li>The block number does not exist or is beyond the chain height</li><li>Index is not a valid hex integer</li><li>Block does have transaction at that index</li></ul> |

#### Integration with Web3

The `eth_getTransactionByBlockNumberAndIndex` method helps developers:

* Build trustless UIs that load transaction lists directly from RPC
* Power dashboards analyzing block activity and congestion
* Support exchanges, wallets, and bridges that track transaction placement
* Generate block-level historical analytics
* Verify transaction ordering for MEV, priority fees, and gas bidding strategies



{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send( "eth_getTransactionByBlockNumberAndIndex",
 ["latest", "0x0"]);    
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
import { arbitrum } from 'viem/chains';

// Create Viem client with GetBlock
const client = createPublicClient({
  chain: arbitrum,
  transport: http('https://go.getblock.us/<ACCESS_TOKEN>'),
});

// Using the method through Viem
async function Call() {
    try {
        // Method-specific Viem implementation
        const result = await client.request({
         method: "eth_getTransactionByBlockNumberAndIndex",
         params: ["latest", "0x0"]
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
