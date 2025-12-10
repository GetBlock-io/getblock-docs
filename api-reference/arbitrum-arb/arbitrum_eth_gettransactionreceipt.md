---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Arbitrum

This method returns the receipt of a transaction by transaction hash.

#### Parameters

| Parameter | Type   | Required | Description                                                                                            |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------------ |
| tx\_hash  | string | yes      | The hash of the transaction to fetch the receipt for. Must be a 32-byte hex string prefixed with `0x`. |

#### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
   "jsonrpc": "2.0",
   "method": "eth_getTransactionReceipt",
    "params": [
        "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"
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
     "method": "eth_getTransactionReceipt",
    "params": [
        "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"
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
    "method": "eth_getTransactionReceipt",
    "params": [
        "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"
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
   "method": "eth_getTransactionReceipt",
    "params": [
        "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"
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
        "blockHash": "0x2caf14773f5c8854de725d9077918a0ec14bd123163d5f9e479b06f201452320",
        "blockNumber": "0x5259c43",
        "contractAddress": null,
        "cumulativeGasUsed": "0xc69229",
        "effectiveGasPrice": "0x5f5e100",
        "from": "0xb8b2522480f850eb198ada5c3f31ac528538d2f5",
        "gasUsed": "0x1993d7",
        "gasUsedForL1": "0x113076",
        "l1BlockNumber": "0x105fbf6",
        "logs": [
            {
                "address": "0x09e18590e8f76b6cf471b3cd75fe1a1a9d2b2c2b",
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x00000000000000000000000009e18590e8f76b6cf471b3cd75fe1a1a9d2b2c2b",
                    "0x000000000000000000000000c873fecbd354f5a56e00e710b90ef4201db2448d"
                ],
                "data": "0x0000000000000000000000000000000000000000000000000000ad0c364098a8",
                "blockNumber": "0x5259c43",
                "transactionHash": "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c",
                "transactionIndex": "0x7",
                "blockHash": "0x2caf14773f5c8854de725d9077918a0ec14bd123163d5f9e479b06f201452320",
                "logIndex": "0x16",
                "removed": false
            }
        ],
        "logsBloom": "0x002000020000100000480000800100000000004000000",
         "status": "0x1",
        "to": "0x5845696f6031bfd57b32e6ce2ddea19a486fa5e5",
        "transactionHash": "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c",
        "transactionIndex": "0x7",
        "type": "0x0"
    }
}
```

#### Reponse Parameter Definition

| Field Name                   | Data Type        | Format                         | Description                                                                                                                                                                                            |
| ---------------------------- | ---------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **transactionHash**          | string           | 32 bytes (0x + 64 hex chars)   | Hash that uniquely identifies the transaction                                                                                                                                                          |
| **transactionIndex**         | string           | Hexadecimal                    | Integer position of the transaction within the block (e.g., "0x66")                                                                                                                                    |
| **blockHash**                | string           | 32 bytes (0x + 64 hex chars)   | Hash of the block where this transaction was included                                                                                                                                                  |
| **blockNumber**              | string           | Hexadecimal                    | Block number where this transaction was included (e.g., "0xeff35f")                                                                                                                                    |
| **from**                     | string           | 20 bytes (0x + 40 hex chars)   | Address of the sender who initiated the transaction                                                                                                                                                    |
| **to**                       | string or null   | 20 bytes (0x + 40 hex chars)   | Address of the receiver. `null` when the transaction is a contract creation transaction                                                                                                                |
| **cumulativeGasUsed**        | string           | Hexadecimal                    | Total amount of gas used when this transaction was executed in the block (sum of gas used by this and all preceding transactions in the same block)                                                    |
| **gasUsed**                  | string           | Hexadecimal                    | Amount of gas used by this specific transaction alone                                                                                                                                                  |
| **effectiveGasPrice**        | string           | Hexadecimal                    | Actual value per gas deducted from sender's account. Before EIP-1559: equals transaction's gas price. After EIP-1559: equals `baseFeePerGas + min(maxFeePerGas - baseFeePerGas, maxPriorityFeePerGas)` |
| **contractAddress**          | string or null   | 20 bytes (0x + 40 hex chars)   | Contract address created if the transaction was a contract creation, otherwise `null`                                                                                                                  |
| **logs**                     | array of objects | -                              | Array of log objects generated by this transaction (events emitted by smart contracts)                                                                                                                 |
| **logs\[].address**          | string           | 20 bytes (0x + 40 hex chars)   | Address of the contract that generated the log                                                                                                                                                         |
| **logs\[].topics**           | array of strings | 32 bytes each                  | Array of indexed log topics (first topic is usually the event signature hash)                                                                                                                          |
| **logs\[].data**             | string           | Hexadecimal                    | Non-indexed log data                                                                                                                                                                                   |
| **logs\[].blockNumber**      | string           | Hexadecimal                    | Block number where this log was created                                                                                                                                                                |
| **logs\[].transactionHash**  | string           | 32 bytes (0x + 64 hex chars)   | Hash of the transaction that created this log                                                                                                                                                          |
| **logs\[].transactionIndex** | string           | Hexadecimal                    | Transaction's position in the block                                                                                                                                                                    |
| **logs\[].blockHash**        | string           | 32 bytes (0x + 64 hex chars)   | Hash of the block containing this log                                                                                                                                                                  |
| **logs\[].logIndex**         | string           | Hexadecimal                    | Position of this log in the block                                                                                                                                                                      |
| **logs\[].removed**          | boolean          | true/false                     | `true` if log was removed due to chain reorganization, `false` if valid                                                                                                                                |
| **logsBloom**                | string           | 256 bytes (0x + 512 hex chars) | Bloom filter for light clients to quickly retrieve related logs                                                                                                                                        |
| **status**                   | string           | "0x0" or "0x1"                 | Transaction status: `"0x1"` for success, `"0x0"` for failure (only for post-Byzantium transactions)                                                                                                    |
| **root**                     | string or null   | 32 bytes                       | Post-transaction state root (only for pre-Byzantium transactions, otherwise `null`)                                                                                                                    |
| **type**                     | string           | Hexadecimal                    | Transaction type: `"0x0"` (legacy), `"0x1"` (EIP-2930), `"0x2"` (EIP-1559), or Arbitrum-specific types (see below)                                                                                     |
| **blobGasUsed**              | string or null   | Hexadecimal                    | Amount of blob gas used for this transaction (only for EIP-4844 blob transactions)                                                                                                                     |
| **blobGasPrice**             | string or null   | Hexadecimal                    | Actual value per gas deducted from sender's account for blob gas (only for EIP-4844 blob transactions)                                                                                                 |

#### Use case

The `eth_getTransactionReceipt` :

* Track the outcome of a transaction (success/failure)
* Retrieve events emitted during contract execution
* Calculate actual gas consumption for analytics or dashboards
* Determine the address of newly created contracts
* Power explorers, wallets, and DeFi dashboards with detailed transaction results

#### Error handling

| Status Code | Error Message    | Cause                                      |
| ----------- | ---------------- | ------------------------------------------ |
| 403         | Forbidden        | Missing or invalid ACCESS\_TOKEN.          |
| -32602      | Invalid argument | <ul><li>Invalid transaction hash</li></ul> |

#### Integration with Web3

The `eth_getTransactionReceipt` method helps developers to:

* Build real-time transaction status trackers
* Power dashboards that display events and gas usage
* Validate smart contract interactions without waiting for confirmations elsewhere
* Support dApps and DeFi interfaces that rely on logs for updates
* Ensure trustless monitoring of transaction results

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from "ethers";
const RPC_URL = "https://go.getblock.us/<ACCESS_TOKEN>";
const provider = new ethers.JsonRpcProvider(RPC_URL);
async function Call() {
  try {
     const result = await provider.send( "eth_getTransactionReceipt",
  [
   "0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"
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
         method: "eth_getTransactionReceipt",
         params: ["0x21be7a93a4531a76b1e15d14059582e6b6f9e36cbdc7a85c23667808f4c78b2c"]});
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
