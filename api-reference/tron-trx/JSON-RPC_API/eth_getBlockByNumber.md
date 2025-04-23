---
description: >-
  eth_getBlockByNumber in Tron: JSON-RPC API Interface for retrieving block
  details by number.
---

# eth\_getBlockByNumber - TRON

## Description

The eth\_getBlockByNumber Web3 method in the Tron protocol is a part of the JSON-RPC API Interface, designed to facilitate seamless blockchain interactions. This method allows users to retrieve detailed information about a specific block by its number. By calling the 'eth\_getBlockByNumber' RPC protocol, developers can access data such as block hash, parent hash, timestamp, transaction count, and more. It supports querying both pending and final blocks, providing a versatile tool for blockchain analysis and development. This method is essential for applications requiring precise block data, ensuring efficient and accurate blockchain operations.

## Supported Networks

The eth\_getBlockByNumber RPC method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters eth\_getBlockByNumber method needs to be executed.

* **Block Number (or Tag)**
  * **Type:** String
  * **Description:** The block number to retrieve, specified as a hexadecimal string. It can also be one of the predefined block tags: "earliest", "latest", or "pending".
  * **Required:** Yes
  * **Supported Values:** A hexadecimal string representing the block number, or one of the predefined tags.
* **Full Transaction Objects**
  * **Type:** Boolean
  * **Description:** If true, returns the full transaction objects; if false, returns only the transaction hashes.
  * **Required:** Yes
  * **Supported Values:** `true` or `false`

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using eth\_getBlockByNumber

Request

```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc' 
--header 'Content-Type: application/json' 
{"jsonrpc": "2.0", "method": "eth_getBlockByNumber", "params": ["0xF9CC56", true], "id": "getblock.io"}
```

Response

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "baseFeePerGas": "0x0",
    "difficulty": "0x0",
    "extraData": "0x",
    "gasLimit": "0x2faf080",
    "gasUsed": "0x0",
    "hash": "0x0000000000f9cc56243898cbe88685678855e07f51c5af91322c225ce3693868",
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0xa7eca7946715c631d37d37a21bc066a5baacad10",
    "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "nonce": "0x0000000000000000",
    "number": "0xf9cc56",
    "parentHash": "0x0000000000f9cc55565a602811f23df6b987fcf4d4c2b3d68ffa28e426f57276",
    "receiptsRoot": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "sha3Uncles": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "size": "0x18a",
    "stateRoot": "0x",
    "timestamp": "0x60b078d0",
    "totalDifficulty": "0x0",
    "transactions": [
      {
        "blockHash": "0x0000000000f9cc56243898cbe88685678855e07f51c5af91322c225ce3693868",
        "blockNumber": "0xf9cc56",
        "from": "0x5a06e76029682e8cf9a0ada9e8d174d45dc52093",
        "gas": "0x0",
        "gasPrice": "0x8c",
        "hash": "0x11adfe477e930beb07a1e452dc4b304b89e1b2ae3e8768a2a3e4b4354d007da2",
        "input": "0x",
        "nonce": "0x0000000000000000",
        "r": "0x5631a521cfdc93afae27985f8c568b848f947d8c6d060da1dd0ca5566a35be9f",
        "s": "0x06c4575dd13c3bb60c5712266564ca9f2ba8b6e459db2bbc7d994848e6154d65",
        "to": "0x45945a2fd879e5989528fd4b9e43338dd484455f",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x1b",
        "value": "0x989680"
      }
```

## Body Parameters

Here is the list of body parameters for the `eth_getBlockByNumber` method:

1. **jsonrpc**: The version of the JSON-RPC protocol, typically "2.0".
2. **id**: An identifier for the request, which helps in matching responses to requests.
3. **result**: Contains the block details:
   * **baseFeePerGas**: The base fee per gas in hexadecimal.
   * **difficulty**: The difficulty of the block in hexadecimal.
   * **extraData**: Additional data included in the block.
   * **gasLimit**: The maximum amount of gas that can be used in the block in hexadecimal.
   * **gasUsed**: The total gas used by all transactions in the block in hexadecimal.
   * **hash**: The hash of the block in hexadecimal.
   * **logsBloom**: The bloom filter for the logs of the block.
   * **miner**: The address of the miner who mined the block.
   * **mixHash**: A hash used in the mining process.
   * **nonce**: A hash that proves the block has been mined.
   * **number**: The block number in hexadecimal.
   * **parentHash**: The hash of the parent block.
   * **receiptsRoot**: The root of the receipts trie of the block.
   * **sha3Uncles**: The SHA3 hash of the uncles data in the block.
   * **size**: The size of the block in bytes in hexadecimal.
   * **stateRoot**: The root of the final state trie of the block.
   * **timestamp**: The time at which the block was mined in hexadecimal.
   * **totalDifficulty**: The total difficulty of the chain until this block in hexadecimal.
   * **transactions**: An array of transactions included in the block:
     * **blockHash**: The hash of the block containing the transaction.
     * **blockNumber**: The block number containing the transaction.
     * **from**: The address of the sender.
     * **gas**: The gas provided by the sender in hexadecimal.
     * **gasPrice**: The gas price provided by the sender in hexadecimal.
     * **hash**: The hash of the transaction.
     * **input**: The data sent along with the transaction.
     * **nonce**: The number of transactions made by the sender prior to this one.
     * **r**: The signature r value.
     * **s**: The signature s value.
     * **to**: The address of the receiver.
     * **transactionIndex**: The index position of the transaction in the block.
     * **type**: The transaction type.
     * **v**: The recovery id.
     * **value**: The value transferred in the transaction in hexadecimal.

## Use Case

Here are some use-cases for the `eth_getBlockByNumber` method:

1. **Historical Data Analysis**: Developers and analysts can use the `eth_getBlockByNumber` method to retrieve detailed information about a specific block on the Ethereum blockchain. This is particularly useful for analyzing historical data, such as transaction patterns, miner behavior, and gas usage over time. By examining blocks at different heights, one can gain insights into network performance and trends.
2. **Transaction Verification**: When building decentralized applications (dApps), it is often crucial to verify the inclusion of transactions within a block. By using `eth_getBlockByNumber`, developers can fetch a block and inspect its list of transactions to confirm whether a particular transaction has been successfully mined and included in the blockchain. This is essential for ensuring the integrity and confirmation of transactions within a dApp.
3. **Blockchain Monitoring and Alerts**: For monitoring the health and status of the Ethereum network, the `eth_getBlockByNumber` method can be used to track the latest blocks being added to the blockchain. By continuously fetching the latest block data, developers can set up systems to detect anomalies, such as unusually long block times or unexpected changes in block size, and generate alerts to notify network administrators or stakeholders of potential issues.

## Code for eth\_getBlockByNumber

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/jsonrpc"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0", "method": "eth_getBlockByNumber", "params": ["0xF9CC56", true], "id": "getblock.io"}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)
```

## Common Errors

Common Errors\
When using the eth\_getBlockByNumber RPC Tron method, the following issues may occur:

* Incorrect Block Number Format: Ensure the block number is provided in hexadecimal format prefixed with "0x". Convert decimal numbers to hexadecimal to avoid this error.
* Network Synchronization Delay: The node may not be fully synchronized, resulting in outdated data. Verify the node's sync status or switch to a fully synchronized node.
* Parameter Mismatch: The method requires an exact match in parameter types; ensure the second parameter is a boolean value to specify full transaction objects.
* Invalid JSON Syntax: Errors in JSON formatting, such as missing commas or incorrect brackets, can lead to request failures. Validate the JSON structure before sending the request.

Using the eth\_getBlockByNumber method in Web3 applications allows developers to retrieve comprehensive block information by specifying a block number, facilitating efficient data retrieval for blockchain analysis and transaction verification. This method is essential for applications that require up-to-date blockchain data, enhancing the reliability and responsiveness of decentralized applications.

### conclusion

The `eth_getBlockByNumber` RPC method is a crucial tool for retrieving detailed information about a specific block on the Ethereum blockchain. By specifying the block number, users can access comprehensive data, including transactions and block metadata, which is essential for developers and analysts alike. While this method is specific to Ethereum, similar functionalities exist in other blockchain platforms like Tron, enabling cross-platform blockchain analysis and integration.
