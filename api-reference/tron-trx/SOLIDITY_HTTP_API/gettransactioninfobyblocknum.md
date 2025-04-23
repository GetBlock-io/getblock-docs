---
description: >-
  Retrieve transaction details by block number with gettransactioninfobyblocknum
  in Tron's HTTP REST API Interface.
---

# gettransactioninfobyblocknum - TRON

## Description

The 'gettransactioninfobyblocknum' method in the Tron protocol allows developers to fetch transaction information for a specific block number via the HTTP REST API. This Web3-compatible REST protocol call returns structured data, including transaction hashes, timestamps, and statuses, streamlining blockchain analysis and integration. Ideal for dApps, explorers, and backend services, 'gettransactioninfobyblocknum' ensures efficient querying of on-chain activity. Learn how to leverage this REST method for seamless Tron network interactions.

## Supported Networks

The gettransactioninfobyblocknum HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

**num** (`int32`):\
Represents the **block number**.\
If no value is provided, the default is:\
`20000000`.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using gettransactioninfobyblocknum

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyblocknum \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '{"num":20000000}'
```

Response

```json
[]
```

## Body Parameters

```
This method does not require any parameters.
```

## Use Case

Here are some use-cases for the `gettransactioninfobyblocknum` method in Web3 programming:

1. **Blockchain Transaction Analysis**\
   Developers can use this method to retrieve all transaction details within a specific block number. This is useful for auditing, tracking token movements, or analyzing transaction patterns for a particular block.
2. **Smart Contract Debugging**\
   When debugging a smart contract, this method helps fetch transaction data (e.g., inputs, gas usage, and execution status) from a specific block, allowing developers to verify contract interactions and identify issues.
3. **Block Explorer Functionality**\
   Applications like block explorers or wallet services can use this method to display transaction histories tied to a specific block, providing users with transparency and real-time blockchain data.

Would you like additional details on any of these use cases?

## Code for gettransactioninfobyblocknum

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactioninfobyblocknum"
headers = {
    "accept": "application/json",
    "content-type": "application/json"
}
payload = {
    "num": 20000000  
}

try:
    response = requests.post(url, headers=headers, json=payload)
    response.raise_for_status()  
    print(response.json())
except requests.exceptions.RequestException as e:
    print(f"Error making request: {e}")
except ValueError as e:
    print(f"Error decoding JSON response: {e}")
```

## Common Errors

**Common Errors**\
When using the `gettransactioninfobyblocknum` REST Tron method, the following issues may occur:

* **Invalid block number**: The specified block number may not exist or be out of range. Ensure the block is confirmed and within the chain's current height.
* **Node synchronization issues**: The node may not be fully synced, leading to missing or incomplete data. Verify the node's sync status before querying.
* **Rate limiting**: Excessive requests can trigger rate limits, resulting in temporary access restrictions. Implement request throttling or use a load-balanced node.

The `gettransactioninfobyblocknum` method is invaluable for Web3 apps, enabling efficient retrieval of transaction details by block number. It simplifies auditing and tracking on-chain activity, making it ideal for explorers, analytics tools, and decentralized applications.

### conclusion

In conclusion, the **gettransactioninfobyblocknum** REST method is a powerful tool for querying transaction details within specific blocks on the **Tron** blockchain. By leveraging this **Tron** RPC call, developers can efficiently retrieve critical transaction data, enhancing transparency and auditability. Whether for analytics or debugging, **gettransactioninfobyblocknum** proves essential for seamless blockchain interactions.
