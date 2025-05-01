---
description: >-
  Retrieve transaction receipt details on the BSC protocol using the
  eth_getTransactionReceipt method via the JSON-RPC API Interface.
---

# eth\_getTransactionReceipt - BNB Smart Chain

{% hint style="success" %}
The RPC method retrieves transaction receipt details on BSC, including status, block number, gas used, and logs, confirming transaction execution.
{% endhint %}

The `eth_getTransactionReceipt` method in the BSC protocol is a crucial part of both the `eth_getTransactionReceipt` Web3 and `eth_getTransactionReceipt` RPC protocol. It retrieves the receipt of a transaction by its hash, providing details such as status, gas used, and logs generated during execution.

Accessible via JSON-RPC, `eth_getTransactionReceipt` is indispensable for verifying transaction outcomes and debugging smart contracts. It returns `null` for pending transactions, ensuring users can efficiently track transaction finality and network interactions with precision and ease.

### Supported Networks

The `eth_getTransactionReceipt` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getTransactionReceipt` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Transaction Hash**
  * **Type**: String
  * **Description**: The hash of the transaction for which the receipt is being requested.
  * **Required**: Yes
  * **Default/Supported Values**: A 32-byte hash represented as a hexadecimal string, prefixed with "0x".

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getTransactionReceipt` :

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

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/{ACCESS_TOKEN}/
{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_getTransactionReceipt` upon a successful call:

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

Here is the list of body parameters for the `eth_getTransactionReceipt` method:

1. **blockHash**: The hash of the block where this transaction was included.
2. **blockNumber**: The block number where this transaction was included.
3. **contractAddress**: The contract address created, if the transaction was a contract creation, otherwise `null`.
4. **cumulativeGasUsed**: The total amount of gas used when this transaction was executed in the block.
5. **effectiveGasPrice**: The effective gas price paid for this transaction.
6. **from**: The address of the sender.
7. **gasUsed**: The amount of gas used by this specific transaction alone.
8. **logs**: An array of log objects, which this transaction generated.
9. **logsBloom**: A bloom filter of logs generated by this transaction.
10. **status**: The status of the transaction, either `0x1` (success) or `0x0` (failure).
11. **to**: The address of the receiver. `null` when it's a contract creation transaction.
12. **transactionHash**: The hash of the transaction.
13. **transactionIndex**: The index position of the transaction in the block.
14. **type**: The type of transaction (e.g., `0x0` for legacy transactions).

### Use Cases

Here are some use-cases for `eth_getTransactionReceipt` method:

1. **Transaction Confirmation**: One of the primary uses of the `eth_getTransactionReceipt` method is to confirm whether a transaction has been successfully mined and included in a block. By retrieving the transaction receipt, developers can check the `status` field to determine if the transaction succeeded or failed. This is crucial for applications that need to ensure that a transaction, such as a token transfer or a smart contract interaction, has been completed successfully before proceeding with further operations.
2. **Gas Usage Analysis**: The `eth_getTransactionReceipt` method provides detailed information about the gas used by a transaction. Developers can use this data to analyze the efficiency of their smart contracts and optimize gas usage. The receipt includes the `gasUsed` field, which indicates the amount of gas consumed by the transaction, allowing developers to identify potentially expensive operations and make necessary adjustments.
3. **Event Logs Retrieval**: Another important use case for `eth_getTransactionReceipt` is to access the logs generated by a transaction. When a smart contract emits events, these are recorded in the transaction receipt's `logs` field. Developers can use this information to trigger off-chain actions, update application states, or monitor specific events within the blockchain. This capability is essential for building interactive and responsive decentralized applications (dApps).

### Code for eth\_getTransactionReceipt

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
  "method": "eth_getTransactionReceipt",
  "params": ["0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a"],
  "id": 1
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "method": "eth_getTransactionReceipt",
  "params": ["0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a"],
  "id": 1
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  if (error.response) {
    console.error("Error:", error.response.status, error.response.data);
  } else {
    console.error("Request failed:", error.message);
  }
});
```
{% endtab %}
{% endtabs %}

### Common Errors

When using the `eth_getTransactionReceipt` JSON-RPC API BSC method, the following issues may occur:

* The transaction hash may be incorrect or malformed, resulting in a null response. Ensure the transaction hash is a valid 32-byte hexadecimal string prefixed with "0x".
* If the transaction is pending, the receipt will not be available. Wait for the transaction to be mined and then re-query for the receipt.
* Network congestion or node issues can lead to timeouts or delays in receiving a response. Consider retrying the request or using a different node provider to mitigate this issue.

Using the `eth_getTransactionReceipt` method in Web3 applications provides critical information about transaction outcomes, including status, gas used, and logs generated. This method is essential for verifying transaction success and debugging smart contract interactions, enhancing the reliability and transparency of decentralized applications.

### Conclusion

The `eth_getTransactionReceipt` method is a crucial JSON-RPC call used to retrieve the status and details of a specific transaction on blockchain networks like Ethereum and BNB Smart Chain (BSC). By providing the transaction hash, users can obtain information such as block number, gas used, and logs, which are essential for verifying transaction success and debugging smart contracts. This makes `eth_getTransactionReceipt` an indispensable tool for developers and users interacting with decentralized applications on BSC and other Ethereum-compatible networks.
