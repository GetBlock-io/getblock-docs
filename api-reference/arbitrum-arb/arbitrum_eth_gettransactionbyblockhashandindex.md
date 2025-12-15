---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON RPC method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex JSON RPC in
  GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Arbitrum

This method returns information about a transaction at a given index within a block, identified by its block hash. This method helps developers inspect specific transactions without having to scan the entire block.

#### Parameters

| Parameter   | Type         | Required | Description                                                                                        |
| ----------- | ------------ | -------- | -------------------------------------------------------------------------------------------------- |
| block\_hash | string       | yes      | The block hash used to locate the block containing the transaction.                                |
| index       | string (hex) | yes      | The transaction index position within the block, encoded as a hex integer (e.g. `"0x0"`, `"0x2"`). |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
       "method": "eth_getTransactionByBlockHashAndIndex",
 "params": [
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
{% endtab %}
{% endtabs %}

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blockHash": "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        "blockNumber": "0x5206d53",
        "from": "0x00000000000000000000000000000000000a4b05",
        "gas": "0x0",
        "gasPrice": "0x0",
        "hash": "0xaf256a9ce8839489dcaa4f9a2740bcd5865b3db34a15434cce29e6bdcf043bc7",
        "input": "0x6bf6a42d0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000105e0460000000000000000000000000000000000000000000000000000000005206d530000000000000000000000000000000000000000000000000000000000000000",
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

#### Use case

The `eth_getTransactionByBlockHashAndIndex` is used to:

* Fetch transactions in the exact order they appear in a block
* Supports debugging by locating problematic transactions in a specific block
* Helps indexers reconstruct historical transaction sequences
* Used by wallets to verify whether their transaction was included and at what position
* Supports analytics tools that measure block activity density

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
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
        method: "eth_getTransactionByBlockHashAndIndex",
 params: [
"0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
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
