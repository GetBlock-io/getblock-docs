---
description: >-
  Access transaction details using eth_getTransactionByBlockNumberAndIndex via the JSON-RPC API Interface in the BSC protocol.
---

# eth_getTransactionByBlockNumberAndIndex

{% hint style="success" %}
The RPC method retrieves a transaction from the Binance Smart Chain by specifying the block number and transaction index.&#x20;
{% endhint %}

The eth_getTransactionByBlockNumberAndIndex Web3 method is a vital component of the BSC protocol, allowing users to retrieve specific transaction details within a block. By using the eth_getTransactionByBlockNumberAndIndex RPC protocol, developers can query transactions by specifying a block number and the transaction index within that block. This method returns comprehensive transaction data, including the hash, sender, receiver, value, and more. It's particularly useful for applications that require precise transaction tracking and analysis. The JSON-RPC interface ensures seamless integration and efficient communication with the blockchain, making it an essential tool for developers working on blockchain-based solutions.

### Supported Networks

The eth_getTransactionByBlockNumberAndIndex REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getTransactionByBlockNumberAndIndex method needs to be executed.

- **Block Number (Required)**
  - **Type**: String
  - **Description**: The block number from which the transaction is to be fetched. It should be provided in hexadecimal format with a "0x" prefix.
  - **Supported Values**: Any valid block number in hexadecimal format.

- **Transaction Index (Required)**
  - **Type**: String
  - **Description**: The index position of the transaction within the block. This is also provided in hexadecimal format with a "0x" prefix.
  - **Supported Values**: Any valid transaction index in hexadecimal format.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getTransactionByBlockNumberAndIndex

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getTransactionByBlockNumberAndIndex",
  "params": ["0xc5043f", "0x0"],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

### Response


```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0xf6e21d834a28d32a340b1bfb721019c3c3b49ae6b0f0a49c9f4c14ca1d82a000",
    "blockNumber": "0xc5043f",
    "from": "0x4982085c9e2f89f2ecb8131eca71afad896e89cb",
    "gas": "0x13c32",
    "gasPrice": "0x4a817c800",
    "hash": "0xeac6fc618eed658e566d23bb8a76fd258ddf075305bae5c5803ae6f83fead55b",
    "input": "0xa9059cbb000000000000000000000000bf2bc9f66955db1c62cfec5a564b3e62c752008a000000000000000000000000000000000000000000000482114f7028102f0000",
    "nonce": "0x1a14c0",
    "to": "0xe56842ed550ff2794f010738554db45e60730371",
    "transactionIndex": "0x0",
    "value": "0x0",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0xcd1ff18127b5177f308529e47d282146269105ce140196ed40e416bf6fc32123",
    "s": "0x7561d811c71860ca9384e2996539628818fecf54a911783295b99b410182fc5b"
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_getTransactionByBlockNumberAndIndex method:

1. blockHash: "0xf6e21d834a28d32a340b1bfb721019c3c3b49ae6b0f0a49c9f4c14ca1d82a000"
2. blockNumber: "0xc5043f"
3. from: "0x4982085c9e2f89f2ecb8131eca71afad896e89cb"
4. gas: "0x13c32"
5. gasPrice: "0x4a817c800"
6. hash: "0xeac6fc618eed658e566d23bb8a76fd258ddf075305bae5c5803ae6f83fead55b"
7. input: "0xa9059cbb000000000000000000000000bf2bc9f66955db1c62cfec5a564b3e62c752008a000000000000000000000000000000000000000000000482114f7028102f0000"
8. nonce: "0x1a14c0"
9. to: "0xe56842ed550ff2794f010738554db45e60730371"
10. transactionIndex: "0x0"
11. value: "0x0"
12. type: "0x0"
13. chainId: "0x38"
14. v: "0x93"
15. r: "0xcd1ff18127b5177f308529e47d282146269105ce140196ed40e416bf6fc32123"
16. s: "0x7561d811c71860ca9384e2996539628818fecf54a911783295b99b410182fc5b"

### Use Cases

Here are some use-cases for eth_getTransactionByBlockNumberAndIndex method:

1. Transaction Analysis: Developers and analysts can use this method to retrieve specific transactions from a block by providing the block number and the transaction index. This is particularly useful when analyzing transaction patterns or verifying the details of a specific transaction within a block.

2. Blockchain Monitoring: This method is essential for applications that monitor blockchain activity in real-time. By fetching transactions based on their position within a block, developers can quickly respond to specific events or actions on the blockchain, such as detecting high-value transactions or monitoring transactions from specific addresses.

3. Data Validation: In scenarios where data integrity and validation are critical, this method allows developers to cross-reference transaction details with their expected outcomes. By accessing transactions directly by their index within a block, developers can ensure that their applications are interacting with the blockchain as intended, maintaining trust and accuracy in decentralized applications.

### Code for eth_getTransactionByBlockNumberAndIndex

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0xf6e21d834a28d32a340b1bfb721019c3c3b49ae6b0f0a49c9f4c14ca1d82a000",
    "blockNumber": "0xc5043f",
    "from": "0x4982085c9e2f89f2ecb8131eca71afad896e89cb",
    "gas": "0x13c32",
    "gasPrice": "0x4a817c800",
    "hash": "0xeac6fc618eed658e566d23bb8a76fd258ddf075305bae5c5803ae6f83fead55b",
    "input": "0xa9059cbb000000000000000000000000bf2bc9f66955db1c62cfec5a564b3e62c752008a000000000000000000000000000000000000000000000482114f7028102f0000",
    "nonce": "0x1a14c0",
    "to": "0xe56842ed550ff2794f010738554db45e60730371",
    "transactionIndex": "0x0",
    "value": "0x0",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0xcd1ff18127b5177f308529e47d282146269105ce140196ed40e416bf6fc32123",
    "s": "0x7561d811c71860ca9384e2996539628818fecf54a911783295b99b410182fc5b"
  }
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_getTransactionByBlockNumberAndIndex JSON-RPC API BSC method, the following issues may occur:  
- Incorrect block number format: Ensure that the block number is provided in hexadecimal format prefixed with "0x". Double-check your input to avoid format-related errors.  
- Index out of range: The provided transaction index may exceed the number of transactions in the block. Verify the block's transaction count before specifying an index.  
- Block not found: If the specified block number does not exist on the blockchain, the method will fail. Confirm the block number's validity by checking the latest block number on the network.  
- Network connectivity issues: If there are problems connecting to the BSC node, the request might time out or fail. Ensure your network connection is stable and that the node endpoint is reachable.

Using the eth_getTransactionByBlockNumberAndIndex method in Web3 applications allows developers to efficiently retrieve specific transactions from a block by specifying the block number and transaction index. This method is particularly useful for applications that require precise transaction data retrieval, enabling more streamlined and targeted data processing.

### conclusion

The eth_getTransactionByBlockNumberAndIndex method in JSON-RPC is a crucial tool for accessing specific transactions within a block on blockchain networks like BSC. By providing the block number and transaction index, users can retrieve detailed transaction data efficiently. This method enhances the ability to interact with and analyze blockchain data, making it a fundamental component for developers working with Ethereum-compatible networks.
