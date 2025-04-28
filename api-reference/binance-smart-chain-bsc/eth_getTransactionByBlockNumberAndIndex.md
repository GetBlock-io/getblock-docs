---
description: >-
  Retrieve transaction details by block number and index using
  eth_getTransactionByBlockNumberAndIndex in the JSON-RPC API Interface on BSC.
---

# eth\_getTransactionByBlockNumberAndIndex - Binance Smart Chain

{% hint style="success" %}
Retrieves a transaction from a specific block number and index on BSC, aiding in transaction tracking and blockchain analysis.
{% endhint %}

The `eth_getTransactionByBlockNumberAndIndex` method in the BSC protocol allows users to retrieve a specific transaction from a block using its block number and transaction index. This method is part of the `eth_getTransactionByBlockNumberAndIndex Web3` functionality, providing developers with a precise way to access transaction details within a block.

Utilizing the `eth_getTransactionByBlockNumberAndIndex RPC protocol`, developers can query the BSC network to obtain transaction information efficiently. This method requires two parameters: the block number and the transaction index, both of which are essential for pinpointing the exact transaction within a block, ensuring accurate data retrieval.

### Supported Networks

The eth\_getTransactionByBlockNumberAndIndex JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getTransactionByBlockNumberAndIndex` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Block Number**
  * **Type:** String (Hexadecimal)
  * **Description:** The block number from which the transaction should be fetched. It is specified in hexadecimal format.
  * **Required:** Yes
  * **Example Value:** `"0xc5043f"` (which is a hexadecimal representation of the block number)
* **Transaction Index**
  * **Type:** String (Hexadecimal)
  * **Description:** The index position of the transaction within the block. It is specified in hexadecimal format.
  * **Required:** Yes
  * **Example Value:** `"0x0"` (which is a hexadecimal representation of the transaction index, indicating the first transaction in the block)

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_getTransactionByBlockNumberAndIndex :

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

**Response**

Below is a sample JSON response returned by eth\_getTransactionByBlockNumberAndIndex upon a successful call:

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

Here is the list of body parameters for `eth_getTransactionByBlockNumberAndIndex` method:

1. **blockHash**: `0xf6e21d834a28d32a340b1bfb721019c3c3b49ae6b0f0a49c9f4c14ca1d82a000`
2. **blockNumber**: `0xc5043f`
3. **from**: `0x4982085c9e2f89f2ecb8131eca71afad896e89cb`
4. **gas**: `0x13c32`
5. **gasPrice**: `0x4a817c800`
6. **hash**: `0xeac6fc618eed658e566d23bb8a76fd258ddf075305bae5c5803ae6f83fead55b`
7. **input**: `0xa9059cbb000000000000000000000000bf2bc9f66955db1c62cfec5a564b3e62c752008a000000000000000000000000000000000000000000000482114f7028102f0000`
8. **nonce**: `0x1a14c0`
9. **to**: `0xe56842ed550ff2794f010738554db45e60730371`
10. **transactionIndex**: `0x0`
11. **value**: `0x0`
12. **type**: `0x0`
13. **chainId**: `0x38`
14. **v**: `0x93`
15. **r**: `0xcd1ff18127b5177f308529e47d282146269105ce140196ed40e416bf6fc32123`
16. **s**: `0x7561d811c71860ca9384e2996539628818fecf54a911783295b99b410182fc5b`

### Use Cases

Here are some use-cases for `eth_getTransactionByBlockNumberAndIndex` method:

1. **Transaction Analysis**: Developers and analysts often need to examine specific transactions within a block to understand contract interactions or to debug issues. By using `eth_getTransactionByBlockNumberAndIndex`, they can retrieve detailed information about a transaction at a specific index in a block, including sender, receiver, value, and data payload. This is particularly useful for auditing and monitoring blockchain activity in decentralized applications (dApps).
2. **Block Verification**: In blockchain development, verifying the integrity and order of transactions within a block is crucial. `eth_getTransactionByBlockNumberAndIndex` allows developers to programmatically fetch transactions by their position in a block, ensuring that the transactions are executed in the correct order. This can be essential for building applications that rely on the precise sequence of transactions, such as those in financial services or supply chain management.
3. **Transaction History Retrieval**: For applications that need to display or log transaction histories, such as cryptocurrency wallets or block explorers, `eth_getTransactionByBlockNumberAndIndex` provides a way to access specific transactions efficiently. By iterating through the indices of transactions in a block, developers can compile a comprehensive history of transactions for user accounts or smart contracts, enhancing transparency and user experience.

### Code for eth\_getTransactionByBlockNumberAndIndex

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

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
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

When using the `eth_getTransactionByBlockNumberAndIndex` JSON-RPC API BSC method, the following issues may occur:

* Incorrect block number format: Ensure that the block number is provided in hexadecimal format with the "0x" prefix. Convert decimal block numbers to hexadecimal to avoid errors.
* Invalid index value: The transaction index should also be in hexadecimal format. Double-check the index value and ensure it is within the range of available transactions in the specified block.
* Network connectivity issues: If the node is not accessible or the network is experiencing issues, you may encounter timeouts or failed requests. Verify your network connection and node status to resolve this.
* Inconsistent data responses: Occasionally, the node may return outdated or inconsistent data due to synchronization delays. Ensure your node is fully synced with the BSC network to get accurate transaction details.

Using the `eth_getTransactionByBlockNumberAndIndex` method in Web3 applications allows developers to efficiently retrieve specific transaction details by referencing a block number and transaction index. This method provides precise access to transaction data, enabling developers to build robust applications that require transaction-level insights and analytics.

### Conclusion

The JSON-RPC method `eth_getTransactionByBlockNumberAndIndex` is used to retrieve a specific transaction from a block on the Ethereum blockchain or compatible networks like BSC by specifying the block number and the transaction index within that block. This method is crucial for developers and users who need precise transaction data for auditing or analysis purposes. By utilizing `eth_getTransactionByBlockNumberAndIndex`, one can efficiently access detailed transaction information, enhancing transparency and data accessibility on platforms like Ethereum and BSC.
