---
description: >-
  Retrieve Tron transaction details with gettransactionbyid using the HTTP REST
  API Interface. Fast, reliable, and developer-friendly.
---

# gettransactionbyid - TRON

## Description

The 'gettransactionbyid' method in the Tron protocol allows developers to fetch detailed transaction data by its unique ID via the HTTP REST API. This Web3-compatible RPC protocol call returns critical information such as block height, timestamp, contract triggers, and fee details, making it essential for blockchain analytics and dApp integration. Whether debugging or auditing, 'gettransactionbyid' ensures seamless access to on-chain transaction records. Ideal for developers building on Tron, this method supports efficient querying within the decentralized ecosystem.

## Supported Networks

The gettransactionbyid HTTP REST API method supports the following network types

* **Mainnet**
* **Testnets**

## Parameters

Here is the list of parameters the `gettransactionbyid` method needs to be executed:

* **`value`** (Required, _string_):\
  A hexadecimal string representing the transaction ID (hash) to query.\
  Example: `"d7a08e7b28755d294bfb1a0dfd3e3540971269534651059dd8c3d3f53e540e8a"`\
  (Must be a valid 64-character SHA-256 hash in hex format.)

This appears to be a JSON-RPC-style request where the parameter is passed in the request body as a key-value pair. No path or query parameters are involved.

### URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using gettransactionbyid

Request

```json
curl --request POST \
     --url https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactionbyid \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
  "value": "d7a08e7b28755d294bfb1a0dfd3e3540971269534651059dd8c3d3f53e540e8a"
}
'
```

Response

```json
{}
```

## Body Parameters

* **ret** (`Result[]`):\
  An array containing the results of the transaction execution.
* **txID** (`string`):\
  The unique identifier (ID) of the transaction.
* **raw\_data** (`object`):\
  The content of the transaction.\
  For detailed structure and fields, refer to the **Transaction** section.
* **signature** (`string[]`):\
  An array of signatures associated with the transaction.
* **raw\_data\_hex** (`string`):\
  The transaction content encoded as a hexadecimal string.

## Use Case

Here are some use-cases for the `gettransactionbyid` method in Web3 programming:

1. **Transaction Verification and Status Checking**\
   Developers can use this method to verify the details of a specific transaction, such as its status (pending, confirmed, or failed), gas fees, and block inclusion. This is useful for applications that need to confirm whether a transaction was successfully executed on the blockchain before proceeding with further actions.
2. **Audit and Debugging**\
   When debugging smart contract interactions or tracking fund transfers, the `gettransactionbyid` method allows developers to retrieve the full transaction details, including input data, sender/receiver addresses, and event logs. This helps in auditing transactions and diagnosing issues in decentralized applications (dApps).
3. **Transaction History and User Notifications**\
   Wallets and dApps can use this method to fetch and display transaction details to users, providing transparency and real-time updates. For example, a DeFi platform might notify users once a swap or deposit transaction is confirmed by querying it using the transaction ID.

These use-cases highlight the importance of `gettransactionbyid` in ensuring transparency, security, and functionality in blockchain-based applications.

## Code for gettransactionbyid

```python
import requests

url = "https://go.getblock.io/<ACCESS-TOKEN>/walletsolidity/gettransactionbyid"
headers = {
    "accept": "application/json",
    "content-type": "application/json"
}
payload = {
    "value": "d7a08e7b28755d294bfb1a0dfd3e3540971269534651059dd8c3d3f53e540e8a"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```

## Common Errors

**Common Errors**\
When using the `gettransactionbyid` RPC Tron method, the following issues may occur:

* **Invalid transaction ID format**: The provided hash is malformed or incorrect. Ensure the transaction ID is a 64-character hexadecimal string (e.g., `d7a08e7b...`).
* **Transaction not found**: The transaction may not exist or hasn't been confirmed on the chain. Verify the ID or check if the transaction was broadcast successfully.
* **Node synchronization issues**: The node may be out of sync, returning stale or incomplete data. Use a reliable, up-to-date node or retry later.

The `gettransactionbyid` method is essential for Web3 apps to fetch precise transaction details, enabling transparency and auditability. By leveraging this RPC call, developers can verify on-chain activity, track payments, or debug interactions efficiently.

### conclusion

The provided JSON response appears to be a transaction hash or identifier, likely retrieved using the **gettransactionbyid RPC** method on the **Tron** blockchain. The **gettransactionbyid** function is a crucial tool for querying detailed transaction data, enabling users to verify and analyze transactions efficiently. By leveraging this **RPC** call, developers and users can seamlessly track transaction statuses, confirmations, and other metadata on the **Tron** network, ensuring transparency and reliability in blockchain operations.
