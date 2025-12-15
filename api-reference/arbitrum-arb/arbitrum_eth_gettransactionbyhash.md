---
description: >-
  Example code for the eth_getTransactionByHash JSON RPC method. Сomplete guide
  on how to use eth_getTransactionByHash JSON RPC in GetBlock Web3
  documentation.
---

# eth\_getTransactionByHash - Arbitrum

This method retrieves the full details of a transaction using its transaction hash. This endpoint is one of the most frequently used methods when tracking transaction progress, debugging smart contracts, or building explorers.

#### Parameters

| Parameter | Type   | Required | Description                                                                                       |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------- |
| tx\_hash  | string | yes      | The hash of the transaction to retrieve. Must be a 32-byte hex encoded string prefixed with `0x`. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getTransactionByHash",
    "params": [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
     "method": "eth_getTransactionByHash",
    "params": [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
    "method": "eth_getTransactionByHash",
    "params": [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
   "method": "eth_getTransactionByHash",
    "params": [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
        "blockHash": "0xd8fe755134d44967f0c919084d3e0ec9b875c476c924f37bff17e5ef37d10d47",
        "blockNumber": "0x5911313",
        "from": "0xe911f7f98ac57cf0c1cc71519d3ba720089381c4",
        "gas": "0x1a02b0",
        "gasPrice": "0x1dcd6500",
        "hash": "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181",
        "input": "0x1801fbe5d5843b1d8c7345812c7ba7257e927b0c5e000000a6ca46b71aa1cdb6545660ad000000004f62e6aa5433274e333035ca261c12397df9febee0782f42749b8e0b",
        "nonce": "0x3c",
        "to": "0xae56c981f9bb8b07e380b209fcd1498c5876fd4c",
        "transactionIndex": "0x1",
        "value": "0x0",
        "type": "0x0",
        "chainId": "0xa4b1",
        "v": "0x14986",
        "r": "0x87ac24f33be39a0bd27fe84a8934aa957610872606e5dbc46d296843a57930df",
        "s": "0x6ad9aba71382c20a528f3310c369b095e15991b352aea3261e2683c2b1288964"
    }
}
```

#### Reponse Parameter Definition

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

#### Use case

The `eth_getTransactionByHash` :

* Track a transaction using only its hash
* Build user dashboards showing live transaction updates
* Debug smart contract interactions using transaction input data
* Power explorers that index and display transaction details
* Determine whether a transaction has been mined or is still pending
* Analyze gas usage patterns and sender behavior

#### Error handling

| Status Code | Error Message    | Cause                                         |
| ----------- | ---------------- | --------------------------------------------- |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.             |
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
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
         method: "eth_getTransactionByHash",
         params: [
        "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"
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
