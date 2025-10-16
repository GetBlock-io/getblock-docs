---
description: >-
  Explore the 'getblock' JSON-RPC API Interface in the Tron protocol for
  retrieving block details by height or ID with parameters and examples
---

# getblock - TRON

## Description

The 'getblock' Web3 method in the Tron protocol allows developers to fetch detailed block information using the getblock RPC protocol. This JSON-RPC API interface supports querying blocks by height or hash, returning data like transactions, timestamps, and block headers.&#x20;

Ideal for blockchain explorers, wallets, and dApps, 'getblock' ensures seamless integration with Tron's decentralized network. Learn how to leverage this endpoint for real-time block analytics and chain synchronization.

## Supported Networks

The getblock RPC method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

* **id\_or\_num** (`string`):\
  This field can be either a **block height** or a **block hash**.\
  If no value is provided, the query will return information about the **latest block** by default.
* **detail** (`boolean`, default: `false`):\
  Determines the level of detail in the response.
  * If set to `true`, the query will return the **full block information**, including both the **header** and **body**.
  * If set to `false`, only the **block header** will be returned.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getblock

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblock \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '{"detail":false}'
```

Response

```json
{
  "blockID": "000000000335f6219b675cfb09a93b99a53729b80b188dcde464eb41625c3d0c",
  "block_header": {
    "raw_data": {
      "number": 53868065,
      "txTrieRoot": "0000000000000000000000000000000000000000000000000000000000000000",
      "witness_address": "41711cf6683d28621ae12030fd541b288c61d682cd",
      "parentHash": "000000000335f620ef6509d67751753763be896f7ac569fc1d1cfa059cecf619",
      "version": 31,
      "timestamp": 1745423034000
    },
    "witness_signature": "218a3429ffaa959ab31887516639b38e55508fff9b9509d44d151371450962de55812a35f22ed352a799def8cc78bc44271f38e8554d5ffe489e8fdf7a18616900"
  }
}
```

## Body Parameters

* **blockID** (`string`):\
  The hash of the block.
* **block\_header.raw\_data.timestamp** (`int64`):\
  The timestamp when the block was produced.
* **block\_header.raw\_data.txTrieRoot** (`string`):\
  The root hash of the transaction Merkle tree for the block.
* **block\_header.raw\_data.parentHash** (`string`):\
  The hash of the parent (previous) block.
* **block\_header.raw\_data.number** (`int64`):\
  The block number (height) in the blockchain.
* **block\_header.raw\_data.witness\_id** (`int64`):\
  The ID of the super representative (SR) that produced the block.
* **block\_header.raw\_data.witness\_address** (`string`):\
  The address of the super representative who produced the block.
* **block\_header.raw\_data.version** (`int32`):\
  The version of the block format.
* **block\_header.raw\_data.accountStateRoot** (`string`):\
  The root hash of the account state tree.
* **block\_header.witness\_signature** (`string`):\
  The cryptographic signature of the super representative (SR) who produced the block.
* **transactions** (`Transaction[]`):\
  A list of transactions included in the block.\
  Each transaction's structure and details can be found by referring to the `gettransactionbyid` documentation.

## Use Cases

Here are some use-cases for the `getBlock` method in Web3 programming:

1. **Block Data Analysis** – Developers use `getBlock` to retrieve detailed information about a specific block, such as its timestamp, transactions, gas used, and miner address. This is useful for analyzing blockchain activity, tracking network performance, or auditing historical data.
2. **Transaction Verification** – By fetching a block and inspecting its transactions, applications can verify whether a particular transaction was included in the blockchain. This is essential for confirming payments, smart contract executions, or other on-chain events.
3. **Synchronization & Indexing** – Services like block explorers, wallets, or decentralized applications (dApps) use `getBlock` to synchronize with the latest blockchain state or index past blocks for querying transaction histories, balances, or contract interactions.

These use cases highlight how `getBlock` serves as a foundational tool for interacting with blockchain data in Web3 development.

## Code for getblock

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblock"
headers = {
    "accept": "application/json",
    "content-type": "application/json"
}
data = {"detail": False}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

## Common Errors

When using the `getblock` RPC Tron method, the following issues may occur:

* **Invalid block identifier**: The provided block number or hash is incorrect or out of range. Ensure the block exists and is formatted correctly (e.g., hex for hashes, integers for heights).
* **Node synchronization issues**: The node may not be fully synced, resulting in missing or outdated block data. Verify the node's sync status and retry if needed.
* **Rate limiting or timeout**: High request volume or network latency can cause failures. Implement retry logic or use a load-balanced node provider for reliability.
* **Insufficient permissions**: The node may restrict access to certain blocks. Check API key permissions or use a public endpoint with the required access.

The `getblock` method is essential for Web3 apps to fetch detailed block data, enabling transaction validation, chain analysis, and smart contract interactions. Its reliability and efficiency make it a cornerstone for developers building on the Tron network.

### Conclusion

In conclusion, **GetBlock** provides a powerful and reliable **RPC** solution for interacting with blockchain networks, including **Tron**. Its seamless integration and robust API make it an ideal choice for developers seeking efficient access to blockchain data. With **GetBlock**, users can easily leverage **Tron** and other networks to build scalable and secure decentralized applications.
