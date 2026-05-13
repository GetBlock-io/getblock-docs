---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Optimism

This method retrieves the transaction receipt, which contains information about the transaction's execution, including status (success/failure), gas used, emitted logs, and the contract address if it was a contract creation. Receipts are only available for mined transactions. On Optimism, the receipt includes additional fields for L1 gas information: l1GasUsed, l1GasPrice, l1Fee, and l1FeeScalar, which are essential for understanding the full transaction cost.

## Parameters

| Parameter       | Type           | Description                | Required |
| --------------- | -------------- | -------------------------- | -------- |
| transactionHash | DATA, 32 Bytes | The hash of a transaction. | Yes      |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'https://go.getblock.us/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{
  "jsonrpc": "2.0",
  "method": "eth_getTransactionReceipt",
  "params": [
    "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
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
    "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
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
    "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
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
  "method": "eth_getTransactionReceipt",
  "params": [
    "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"
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
        "blobGasUsed": "0x9c40",
        "blockHash": "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467",
        "blockNumber": "0x9087826",
        "contractAddress": null,
        "cumulativeGasUsed": "0x8829cf",
        "daFootprintGasScalar": "0x190",
        "effectiveGasPrice": "0x15f",
        "from": "0xcde377407d9b819efc0e185aa2d442194c2233e6",
        "gasUsed": "0xbdbfe",
        "l1BaseFeeScalar": "0x146b",
        "l1BlobBaseFee": "0xa92fc3",
        "l1BlobBaseFeeScalar": "0xf79c5",
        "l1Fee": "0xae8f1995",
        "l1GasPrice": "0xcdb8f78",
        "l1GasUsed": "0x640",
        "logs": [
            {
                "address": "0x4200000000000000000000000000000000000006",
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000cde377407d9b819efc0e185aa2d442194c2233e6",
                    "0x0000000000000000000000004aed348173bbf17dfaeb91598321904c9323d36e"
                ],
                "data": "0x0000000000000000000000000000000000000000000000003c9217e5e606acd8",
                "blockNumber": "0x9087826",
                "transactionHash": "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685",
                "transactionIndex": "0x23",
                "blockHash": "0x3e4a6a9efc7e4db75269430a5683ae3c7505ab0848d7814bd1c73c76e6882467",
                "blockTimestamp": "0x6a04ca05",
                "logIndex": "0xa0",
                "removed": false
            },
  ],
        "logsBloom": "0x440002000000000000001002020000000000000040000000000400004001090000000000200000000000001000001000004000000000000006000000002000000000000000000002000000080000000000000002000000000000000000000000100000000200000020800001012018000000000008040000400000100000000000000000080000000000000000000000000080000420000000000000100000008200000008000020000000000000000020000000008000080002000000000080000000020000000100000a0008000080180000000080000800000000000024000014080000040000200000000000002000000000000000000400010000000800",
        "status": "0x1",
        "to": "0x4aed348173bbf17dfaeb91598321904c9323d36e",
        "transactionHash": "0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685",
        "transactionIndex": "0x23",
        "type": "0x2"
    }
}
```
{% endcode %}

## Response Parameters

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

## Use Case

The eth\_getTransactionReceipt method is commonly used for:

* **Transaction confirmation**
* **Event log processing**
* **Contract deployment verification**
* **Gas usage analysis**
* **L1 fee calculation**

#### Error handling

| Status Code | Error Message    | Cause                                      |
| ----------- | ---------------- | ------------------------------------------ |
| 404         | Not Found        | Missing or invalid ACCESS\_TOKEN.          |
| -32602      | Invalid argument | <ul><li>Invalid transaction hash</li></ul> |

#### Integration with Web3

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
{% code overflow="wrap" %}
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
         method: "eth_getTransactionReceipt",
         params: ["0xca5a3c6bcb4a1ddf5e762687816b82dbcf869b8c5a4d7301b65f33f8b0f53685"]});
        console.log('Result:', result);
        return result;
    } catch (error) {
        console.error('Viem Error:', error);
        throw error;
    }
}

```
{% endcode %}
{% endtab %}
{% endtabs %}
