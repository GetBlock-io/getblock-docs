---
description: >-
  Retrieve transaction details using eth_getTransactionByHash in the JSON-RPC API Interface on BSC.
---

# eth_getTransactionByHash

{% hint style="success" %}
Retrieves transaction details on BSC using a transaction hash, providing status, block number, sender, recipient, and value information.&#x20;
{% endhint %}

The eth_getTransactionByHash Web3 method allows users to obtain detailed information about a specific transaction on the Binance Smart Chain (BSC) by providing the transaction hash. This method is part of the eth_getTransactionByHash RPC protocol, which facilitates communication between decentralized applications and the blockchain. By calling this method, users can access various transaction details, including block number, sender and receiver addresses, gas price, and transaction status. This is essential for developers and users who need to verify transaction data or debug blockchain interactions. The method ensures efficient retrieval of transaction data, enhancing the transparency and reliability of blockchain operations.

### Supported Networks

The eth_getTransactionByHash REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getTransactionByHash method needs to be executed:

- **Parameter**: Transaction Hash
  - **Required/Optional**: Required
  - **Type**: String
  - **Description**: The hash of the transaction you want to retrieve. This is a unique identifier for the transaction within the Ethereum blockchain.
  - **Default/Supported Values**: A 32-byte hash, typically represented as a hexadecimal string prefixed with "0x".

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getTransactionByHash

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getTransactionByHash",
  "params": ["0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a"],
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
    "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
    "blockNumber": "0x172522c",
    "from": "0xa2c2ea849a07383684eed1123446d2325a56346c",
    "gas": "0x2cbe4",
    "gasPrice": "0x12a05f200",
    "hash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
    "input": "0x7ff36ab500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c0000000000000000000000000000000000000000000000000000000063ab323b0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000829a20ec98520f9878f49c311565cf4a86ea7cd1",
    "nonce": "0x1695",
    "to": "0x10ed43c718714eb63d5aa57b78b54704e256024e",
    "transactionIndex": "0x17",
    "value": "0xde0b6b3a7640000",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0x44f3adbcbce9135114f7bc5f589f8635f05e9d7339c82f072d1a90f45acb5975",
    "s": "0x52e7b937325e03f3cd923a87c1caa2e2d83ac0a0b9db8c6f2c192c91ae4cadc0"
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_getTransactionByHash method:

1. blockHash: "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd"
2. blockNumber: "0x172522c"
3. from: "0xa2c2ea849a07383684eed1123446d2325a56346c"
4. gas: "0x2cbe4"
5. gasPrice: "0x12a05f200"
6. hash: "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a"
7. input: "0x7ff36ab500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c0000000000000000000000000000000000000000000000000000000063ab323b0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000829a20ec98520f9878f49c311565cf4a86ea7cd1"
8. nonce: "0x1695"
9. to: "0x10ed43c718714eb63d5aa57b78b54704e256024e"
10. transactionIndex: "0x17"
11. value: "0xde0b6b3a7640000"
12. type: "0x0"
13. chainId: "0x38"
14. v: "0x93"
15. r: "0x44f3adbcbce9135114f7bc5f589f8635f05e9d7339c82f072d1a90f45acb5975"
16. s: "0x52e7b937325e03f3cd923a87c1caa2e2d83ac0a0b9db8c6f2c192c91ae4cadc0"

### Use Cases

Here are some use-cases for eth_getTransactionByHash method:

1. **Transaction Status Verification**: Developers can use this method to verify the status of a transaction by retrieving its details using the transaction hash. This is particularly useful in scenarios where a user needs confirmation that their transaction has been successfully mined and included in a block, ensuring that the intended operations, such as token transfers or contract interactions, have been executed on the blockchain.

2. **Transaction Monitoring and Analytics**: This method can be employed to monitor transactions for analytical purposes. By fetching transaction details, developers can gather data such as gas used, sender and receiver addresses, and transaction value. This information can be used to analyze network activity, track transaction trends, or even debug issues related to transaction execution.

3. **User Notifications and Alerts**: In applications where user engagement is crucial, developers can utilize this method to send notifications or alerts to users about the status of their transactions. For instance, once a transaction is confirmed, the application can notify the user, enhancing the user experience by keeping them informed about the progress and completion of their transactions.

### Code for eth_getTransactionByHash

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
    "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
    "blockNumber": "0x172522c",
    "from": "0xa2c2ea849a07383684eed1123446d2325a56346c",
    "gas": "0x2cbe4",
    "gasPrice": "0x12a05f200",
    "hash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
    "input": "0x7ff36ab500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c0000000000000000000000000000000000000000000000000000000063ab323b0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000829a20ec98520f9878f49c311565cf4a86ea7cd1",
    "nonce": "0x1695",
    "to": "0x10ed43c718714eb63d5aa57b78b54704e256024e",
    "transactionIndex": "0x17",
    "value": "0xde0b6b3a7640000",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0x44f3adbcbce9135114f7bc5f589f8635f05e9d7339c82f072d1a90f45acb5975",
    "s": "0x52e7b937325e03f3cd923a87c1caa2e2d83ac0a0b9db8c6f2c192c91ae4cadc0"
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
When using the eth_getTransactionByHash JSON-RPC API BSC method, the following issues may occur:  
- Invalid transaction hash: Ensure that the transaction hash is a valid 32-byte hexadecimal string prefixed with '0x'. Double-check for any typographical errors in the hash.  
- Transaction not found: If the transaction hash does not exist on the blockchain, it may be due to an incorrect hash or the transaction not yet being mined. Verify the hash and wait for the transaction to be confirmed.  
- Network connectivity issues: Connection problems with your BSC node can lead to failed requests. Ensure your node is running and properly connected to the network.  
- Rate limiting: Excessive requests to a public BSC node may result in rate limiting. Consider setting up a dedicated node or using a service with higher rate limits for consistent access.

The eth_getTransactionByHash method is a crucial tool in Web3 applications, allowing developers to retrieve detailed information about specific transactions. This functionality enables efficient transaction tracking and verification, enhancing the transparency and reliability of decentralized applications.

### conclusion

The eth_getTransactionByHash method in JSON-RPC is a crucial tool for retrieving detailed information about a specific transaction using its hash on blockchain networks like Ethereum or BSC. By providing the transaction hash, this method allows users to access data such as the transaction's status, block number, and involved addresses, thereby facilitating efficient blockchain analysis and monitoring.
