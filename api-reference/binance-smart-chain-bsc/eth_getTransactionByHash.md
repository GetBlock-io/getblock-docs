---
description: >-
  Retrieve transaction details by hash using eth_getTransactionByHash in the BSC
  protocol via the JSON-RPC API Interface.
---

# eth\_get TransactionByHash - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves transaction details on Binance Smart Chain using a specific transaction hash, providing information like block number, sender, and receiver.
{% endhint %}

The `eth_getTransactionByHash` method in the BSC protocol allows users to retrieve detailed information about a specific transaction using its unique hash. As part of the `eth_getTransactionByHash Web3` functionality, it returns transaction details such as block hash, sender, recipient, and value transferred, enabling developers to track and verify transactions efficiently.

In the `eth_getTransactionByHash RPC protocol`, this method is crucial for querying transaction data on the blockchain. By providing the transaction hash as a parameter, users receive a JSON object with comprehensive transaction attributes, ensuring seamless integration into blockchain applications and enhancing transaction monitoring capabilities.

### Supported Networks

The eth\_getTransactionByHash JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getTransactionByHash` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Transaction Hash**
  * **Type:** String
  * **Description:** The hash of the transaction you want to retrieve.
  * **Required:** Yes
  * **Default/Supported Values:** Must be a valid transaction hash in hexadecimal format, prefixed with "0x".

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_getTransactionByHash :

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

**Response**

Below is a sample JSON response returned by eth\_getTransactionByHash upon a successful call:

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

Here is the list of body parameters for `eth_getTransactionByHash` method:

1. **blockHash**: "0x9a2b2dc968830767a5b59ff82ee865b1de90127a8a17398928bacf1dfc5170bd"
2. **blockNumber**: "0x172522c"
3. **from**: "0xa2c2ea849a07383684eed1123446d2325a56346c"
4. **gas**: "0x2cbe4"
5. **gasPrice**: "0x12a05f200"
6. **hash**: "0x0ff2022252b448cb86c90653ac26f432669f1dc29c74f3586e143ce1e386c00a"
7. **input**: "0x7ff36ab500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000a2c2ea849a07383684eed1123446d2325a56346c0000000000000000000000000000000000000000000000000000000063ab323b0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c000000000000000000000000829a20ec98520f9878f49c311565cf4a86ea7cd1"
8. **nonce**: "0x1695"
9. **to**: "0x10ed43c718714eb63d5aa57b78b54704e256024e"
10. **transactionIndex**: "0x17"
11. **value**: "0xde0b6b3a7640000"
12. **type**: "0x0"
13. **chainId**: "0x38"
14. **v**: "0x93"
15. **r**: "0x44f3adbcbce9135114f7bc5f589f8635f05e9d7339c82f072d1a90f45acb5975"
16. **s**: "0x52e7b937325e03f3cd923a87c1caa2e2d83ac0a0b9db8c6f2c192c91ae4cadc0"

### Use Cases

Here are some use-cases for `eth_getTransactionByHash` method:

1. **Transaction Verification**: In Web3 programming, ensuring the integrity and completion of transactions is critical. By using the `eth_getTransactionByHash` method, developers can retrieve the transaction details using its hash. This allows them to verify whether a transaction was successfully mined and included in the blockchain, check the status, and confirm the details such as sender, receiver, and amount transferred.
2. **Audit and Monitoring**: For applications that require auditing or monitoring of blockchain transactions, the `eth_getTransactionByHash` method provides a way to fetch transaction data for logging or analysis. This can be particularly useful for compliance purposes, where businesses need to maintain records of all transactions involving their smart contracts.
3. **User Notifications**: In decentralized applications (dApps), it is often necessary to inform users about the status of their transactions. By using the `eth_getTransactionByHash` method, developers can implement features that notify users when their transactions have been confirmed or if there were any issues, enhancing the user experience and trust in the application.

### Code for eth\_getTransactionByHash

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

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
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

When using the `eth_getTransactionByHash` JSON-RPC API BSC method, the following issues may occur:

* **Transaction Hash Not Found**: If the transaction hash is incorrect or the transaction is not yet mined, the method may return null. Verify the transaction hash and ensure the transaction has been confirmed on the blockchain.
* **Network Latency**: Due to network congestion or latency, the response might be delayed. Consider implementing retry logic or increasing the timeout duration in your application.
* **Invalid JSON-RPC Request**: An improperly formatted JSON-RPC request can lead to errors. Double-check the JSON structure, ensuring that the method name and parameters are correctly specified.
* **Node Synchronization Issues**: If the node you are querying is not fully synchronized with the network, it might not return the most recent transaction data. Use a reliable and fully synced node provider.

Using the `eth_getTransactionByHash` method in Web3 applications provides a straightforward way to retrieve detailed information about specific transactions by their hash. This functionality is crucial for tracking transaction statuses, debugging, and ensuring the integrity of blockchain interactions in decentralized applications.

### Conclusion

The JSON-RPC method `eth_getTransactionByHash` is utilized to retrieve detailed information about a specific transaction using its hash on blockchain networks such as Ethereum and Binance Smart Chain (BSC). By providing the transaction hash in the `params` field, users can access comprehensive transaction data, which is crucial for verifying and tracking blockchain activities. This method is essential for developers and users interacting with Ethereum or BSC, as it allows for precise transaction auditing and analysis.
