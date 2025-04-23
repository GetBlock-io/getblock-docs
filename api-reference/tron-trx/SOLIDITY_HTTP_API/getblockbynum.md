---
description: >-
  Retrieve Tron block details by number with 'getblockbynum' via the HTTP REST
  API Interface. Fast, reliable blockchain data access.
---

# getblockbynum - TRON

## Description

The 'getblockbynum' method in the Tron protocol allows developers to fetch block data by block number using the HTTP REST API. This Web3-compatible RPC call returns structured block information, including transactions, timestamps, and hashes, enabling seamless integration with dApps and blockchain analytics. The 'getblockbynum REST protocol' ensures efficient, low-latency access to Tron's decentralized ledger, supporting both full and lightweight clients. Ideal for explorers, wallets, and smart contract platforms requiring real-time block verification. Documentation includes request/response examples for easy implementation.

## Supported Networks

The getblockbynum HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters the method needs to be executed:

* num  &#x20;\- int32

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using getblockbynum

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbynum \
     --header 'accept: application/json' \
     --header 'content-type: application/json'
```

Response

```json
{
  "Error": "class org.tron.core.services.http.JsonFormat$ParseException : 1:1: Expected \"{\"."
}
```

## Body Parameters

This method does not require any parameters.

## Use Case

Here are some use-cases for the `getblockbynum` method in Web3 programming:

1. **Blockchain Data Analysis**\
   Developers and analysts use `getblockbynum` to retrieve specific blocks by their block number, enabling them to study transaction patterns, gas usage, or smart contract interactions. This is useful for auditing, optimizing dApps, or tracking historical activity on the blockchain.
2. **Synchronizing Off-Chain Databases**\
   Applications that rely on indexed blockchain data (e.g., explorers, analytics dashboards) can use this method to fetch blocks sequentially and sync them with off-chain databases. This ensures data consistency for queries without relying on full nodes.
3. **Smart Contract Event Verification**\
   When validating events emitted by smart contracts, `getblockbynum` helps fetch the exact block containing the transaction. This is critical for dispute resolution, proof-of-existence checks, or verifying timestamps in decentralized applications.

This method is particularly valuable for scenarios requiring precise block-level data retrieval.

## Code for getblockbynum

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/getblockbynum"
headers = {
    "accept": "application/json",
    "content-type": "application/json"
}

response = requests.post(url, headers=headers)
print(response.json())
```

## Common Errors

**Common Errors**\
When using the `getblockbynum` RPC Tron method, the following issues may occur:

* **Invalid block number**: If the specified block number is negative or exceeds the current chain height, the request will fail. Always validate the block number against the latest chain height before querying.
* **Node synchronization issues**: If the node is not fully synced, it may return incomplete or outdated block data. Ensure your connected node is synchronized with the Tron mainnet or testnet.
* **Rate limiting or timeout**: High request volumes or slow node responses can lead to timeouts. Implement retry logic or use a load-balanced node provider for reliability.

The `getblockbynum` method is essential for Web3 apps needing precise block data, enabling efficient blockchain analysis, transaction verification, and smart contract interactions. Its simplicity and direct access to block details make it a cornerstone for developers building on Tron.

### conclusion

The provided JSON request appears to be a query for address details on the Tron blockchain, likely using an RPC call. To further explore blockchain data, you might use the **getblockbynum RPC** method on **Tron**, which retrieves block information by its number. This is useful for analyzing transactions, verifying blocks, or auditing the chain. The **getblockbynum** function is a key tool for developers interacting with Tron's decentralized ecosystem, enabling precise block-level queries for smart contracts, dApps, or analytics.
