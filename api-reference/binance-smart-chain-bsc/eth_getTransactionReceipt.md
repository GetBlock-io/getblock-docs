---
description: >-
  Retrieve transaction receipts on BSC using eth_getTransactionReceipt through the JSON-RPC API Interface for efficient blockchain data access.
---

# eth_getTransactionReceipt

{% hint style="success" %}
Retrieves transaction receipt on Binance Smart Chain, providing status, block details, gas used, and logs for executed transactions.&#x20;
{% endhint %}

The eth_getTransactionReceipt Web3 method is a crucial tool in the BSC protocol for developers needing detailed transaction insights. This method, part of the eth_getTransactionReceipt RPC protocol, provides comprehensive transaction receipt data, including status, gas used, and logs, enabling users to verify transaction outcomes efficiently. When invoked, it returns information about a transaction by its hash, offering insights into execution results and potential errors. Designed for seamless integration, this method supports developers in building robust blockchain applications by providing reliable access to transaction details. Utilize this method within the JSON-RPC API Interface to enhance your application's interaction with the Binance Smart Chain, ensuring accurate and timely transaction data retrieval.

### Supported Networks

The eth_getTransactionReceipt REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getTransactionReceipt method needs to be executed:

- **Transaction Hash**
  - **Type:** String
  - **Description:** The hash of the transaction for which the receipt is being requested.
  - **Required/Optional:** Required
  - **Default/Supported Values:** A valid hexadecimal string representing a transaction hash.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getTransactionReceipt

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getTransactionReceipt",
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
    "contractAddress": null,
    "cumulativeGasUsed": "0x161c18",
    "effectiveGasPrice": "0x12a05f200",
    "from": "0xa2c2ea849a07383684eed1123446d2325a56346c",
    "gasUsed": "0x2661a",
    "logs": [
      {
        "address": "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
        "topics": [
          "0xe1fffcc4923d04b559f4d29a8bfc6cda04eb5b0d3c460751c2402c5c5cc9109c",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x27",
        "removed": false
      },
      {
        "address": "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x28",
        "removed": false
      },
      {
        "address": "0x829a20ec98520f9878f49c311565cf4a86ea7cd1",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef",
          "0x00000000000000000000000004e149ed9f356099d235b8402ce7c31f9f207374"
        ],
        "data": "0x000000000000000000000000000000000000000000000193e4dc8b698a81fbb4",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x29",
        "removed": false
      },
      {
        "address": "0x829a20ec98520f9878f49c311565cf4a86ea7cd1",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef",
          "0x000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c"
        ],
        "data": "0x0000000000000000000000000000000000000000000025dd74ad11e4fc2f98e8",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2a",
        "removed": false
      },
      {
        "address": "0xbc930e661334d958e7bda3b126ce0666c353c4ef",
        "topics": [
          "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
        ],
        "data": "0x0000000000000000000000000000000000000000002483f3facf89df82e7549c00000000000000000000000000000000000000000000000cdeb24eaa7da294cc",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2b",
        "removed": false
      },
      {
        "address": "0xbc930e661334d958e7bda3b126ce0666c353c4ef",
        "topics": [
          "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e",
          "0x000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c"
        ],
        "data": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000277159899d4e86b1949c0000000000000000000000000000000000000000000000000000000000000000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2c",
        "removed": false
      }
    ],
    "logsBloom": "0x00200200000000000000000080000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000006000000008000000201000001000000000000400008000000000000000000000000000000000000000000000000020000000000010000000040000000000080000000000000000000000240001100000088000004000000000000000000000000000000000020000000000000000000000000000400100000000000002000000000000000000000000000000000200001000000000000080000000000000000000000000000000000000000010000000400000000000000001",
    "status": "0x1",
    "to": "0x10ed43c718714eb63d5aa57b78b54704e256024e",
    "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
    "transactionIndex": "0x17",
    "type": "0x0"
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_getTransactionReceipt method:

1. `blockHash`: The hash of the block containing the transaction.
2. `blockNumber`: The number of the block containing the transaction.
3. `contractAddress`: The contract address created, if the transaction was a contract creation, otherwise null.
4. `cumulativeGasUsed`: The total amount of gas used when the transaction was executed in the block.
5. `effectiveGasPrice`: The actual gas price paid for the transaction.
6. `from`: The address of the sender.
7. `gasUsed`: The amount of gas used by this specific transaction alone.
8. `logs`: An array of log objects generated by this transaction.
9. `logsBloom`: A bloom filter for the logs generated by the transaction.
10. `status`: The status of the transaction (1 for success, 0 for failure).
11. `to`: The address of the receiver, or null if it was a contract creation transaction.
12. `transactionHash`: The hash of the transaction.
13. `transactionIndex`: The index position of the transaction in the block.
14. `type`: The type of the transaction.

### Use Cases

Here are some use-cases for eth_getTransactionReceipt method in Web3 programming:

1. **Transaction Confirmation**: This method is essential for confirming whether a transaction has been successfully processed on the Ethereum blockchain. Developers can use it to check the status of a transaction, including whether it was successful or if it failed due to an error such as running out of gas. This is crucial for applications that need to verify the completion of transactions before proceeding with subsequent operations.

2. **Gas Usage Analysis**: By retrieving the transaction receipt, developers can analyze the amount of gas used by a particular transaction. This information is valuable for optimizing smart contracts and ensuring cost-effective operations. By understanding gas usage patterns, developers can make informed decisions about contract modifications or transaction batching to reduce costs.

3. **Event Logging and Monitoring**: The transaction receipt provides access to logs generated by smart contract events. Developers can use this information to monitor specific events emitted during the execution of a transaction. This capability is useful for building applications that react to certain blockchain events, such as updating user interfaces or triggering off-chain processes in response to on-chain activities.

### Code for eth_getTransactionReceipt

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
    "contractAddress": null,
    "cumulativeGasUsed": "0x161c18",
    "effectiveGasPrice": "0x12a05f200",
    "from": "0xa2c2ea849a07383684eed1123446d2325a56346c",
    "gasUsed": "0x2661a",
    "logs": [
      {
        "address": "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
        "topics": [
          "0xe1fffcc4923d04b559f4d29a8bfc6cda04eb5b0d3c460751c2402c5c5cc9109c",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x27",
        "removed": false
      },
      {
        "address": "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x28",
        "removed": false
      },
      {
        "address": "0x829a20ec98520f9878f49c311565cf4a86ea7cd1",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef",
          "0x00000000000000000000000004e149ed9f356099d235b8402ce7c31f9f207374"
        ],
        "data": "0x000000000000000000000000000000000000000000000193e4dc8b698a81fbb4",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x29",
        "removed": false
      },
      {
        "address": "0x829a20ec98520f9878f49c311565cf4a86ea7cd1",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000bc930e661334d958e7bda3b126ce0666c353c4ef",
          "0x000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c"
        ],
        "data": "0x0000000000000000000000000000000000000000000025dd74ad11e4fc2f98e8",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2a",
        "removed": false
      },
      {
        "address": "0xbc930e661334d958e7bda3b126ce0666c353c4ef",
        "topics": [
          "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
        ],
        "data": "0x0000000000000000000000000000000000000000002483f3facf89df82e7549c00000000000000000000000000000000000000000000000cdeb24eaa7da294cc",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2b",
        "removed": false
      },
      {
        "address": "0xbc930e661334d958e7bda3b126ce0666c353c4ef",
        "topics": [
          "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
          "0x00000000000000000000000010ed43c718714eb63d5aa57b78b54704e256024e",
          "0x000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c"
        ],
        "data": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000277159899d4e86b1949c0000000000000000000000000000000000000000000000000000000000000000",
        "blockNumber": "0x172522c",
        "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
        "transactionIndex": "0x17",
        "blockHash": "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd",
        "logIndex": "0x2c",
        "removed": false
      }
    ],
    "logsBloom": "0x00200200000000000000000080000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000006000000008000000201000001000000000000400008000000000000000000000000000000000000000000000000020000000000010000000040000000000080000000000000000000000240001100000088000004000000000000000000000000000000000020000000000000000000000000000400100000000000002000000000000000000000000000000000200001000000000000080000000000000000000000000000000000000000010000000400000000000000001",
    "status": "0x1",
    "to": "0x10ed43c718714eb63d5aa57b78b54704e256024e",
    "transactionHash": "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a",
    "transactionIndex": "0x17",
    "type": "0x0"
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
When using the eth_getTransactionReceipt JSON-RPC API BSC method, the following issues may occur:  
- Transaction hash not found: This can occur if the transaction has not yet been mined or if an incorrect hash is provided. Ensure the transaction has been confirmed and verify the hash for accuracy.  
- Null response: A null response indicates the transaction is pending or does not exist. Check the transaction status and confirm the hash is correct.  
- Network connectivity issues: Failure to connect to the BSC network can lead to errors. Ensure your node is properly connected and synchronized with the network.  
- Rate limit exceeded: Excessive requests to the BSC node can result in rate limiting. Implement request throttling and optimize your application's API usage to avoid this issue.  

Using the eth_getTransactionReceipt method provides developers with valuable transaction status information, enabling them to verify successful execution and gather data such as gas usage and logs. This functionality is essential for building responsive and reliable Web3 applications, as it allows for real-time transaction monitoring and error handling.

### conclusion

The eth_getTransactionReceipt method in JSON-RPC is a crucial tool for obtaining transaction receipts on blockchain networks such as Ethereum and BSC. By using this method, developers can access detailed information about a transaction, including its status, block number, and any logs generated. This functionality is essential for verifying transaction outcomes and debugging smart contract interactions on networks like BSC.
