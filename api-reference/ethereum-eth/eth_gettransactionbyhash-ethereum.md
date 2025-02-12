---
description: >-
  Retrieve transaction details by transaction hash using
  eth_getTransactionByHash. Essential for tracking and verifying individual
  transactions on the Ethereum blockchain.
---

# eth\_getTransactionByHash - Ethereum

{% hint style="success" %}
This method returns transaction information for the specified transaction hash, allowing developers to retrieve detailed data about specific transactions
{% endhint %}

The Ethereum eth\_getTransactionByHash method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_getTransactionByHash RPC Ethereum method is crucial for tracking and verifying individual transactions.

### Supported Networks

The eth\_getTransactionByHash RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 32-byte hash transaction.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": [
        "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the transaction details for the specified hash. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "accessList": [],
        "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
        "blockNumber": "0x107d7b0",
        "chainId": "0x1",
        "from": "0x901c7c311d39e0b26257219765e71e8db3107a81",
        "gas": "0x31d74",
        "gasPrice": "0xb9029a7ea",
        "hash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
        "input": "0x3593564c000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000646701fb000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000a968163f0a57b400000000000000000000000000000000000000000000000000000000000010326d79400000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002bb7135877cd5d40aa3b086ac6f21c51bbafbbb41f002710dac17f958d2ee523a2206206994597c13d831ec7000000000000000000000000000000000000000000",
        "maxFeePerGas": "0xf22a22912",
        "maxPriorityFeePerGas": "0x5f5e100",
        "nonce": "0x4",
        "r": "0xef566fc229bb0a10eee5f99c9cabe47f0f20ebaa6d16e4f7b90ee144086b21e9",
        "s": "0x109de5d9baca8daeee1ce1b7d1a304e223d07b1420b37704e675ccffd364a4dc",
        "to": "0xef1c6e67703c7bd7107eed8303fbe6ec2554bf6b",
        "transactionIndex": "0xfc",
        "type": "0x2",
        "v": "0x0",
        "value": "0x0"
    }
}
```

### Response Description

* accessList: A list of addresses and storage keys the transaction accesses (EIP-2930).
* blockHash: The hash of the block containing the transaction.
* blockNumber: The number of the block containing the transaction.
* chainId: The chain ID of the Ethereum network.
* from: The address of the sender.
* gas: The gas provided by the sender.
* gasPrice: The gas price provided by the sender in Wei.
* hash: The hash of the transaction.
* input: The input data for the transaction.
* maxFeePerGas: Maximum fee per gas that the sender is willing to pay.
* maxPriorityFeePerGas: Maximum priority fee per gas.
* nonce: The number of transactions made by the sender prior to this one.
* r, s, v: Components of the transaction signature.
* to: The address of the receiver.
* transactionIndex: The index position of the transaction within the block.
* type: The transaction type.
* value: The value transferred in Wei.

### Use Case

The eth\_getTransactionByHash method is particularly useful for retrieving detailed information about a specific transaction, which can be critical for verifying and tracking transactions on the Ethereum blockchain. In case of an eth\_getTransactionByHash error, developers should verify that the provided transaction hash is correct and corresponds to an existing transaction. An eth\_getTransactionByHash example can help developers understand the correct usage of this method.

### Code Example

You can also make requests to the eth\_getTransactionByHash method programmatically using Python. Below is an example using the requests library:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API URL and access token
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}

# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByHash",
    "params": [
        "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c"
    ],
    "id": "getblock.io"
}

# Send the POST request
response = requests.post(url, headers=headers, data=json.dumps(data))

# Parse the JSON response
response_data = response.json()

# Print the result
print(json.dumps(response_data, indent=4))
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

// Replace YOUR-API-KEY with your actual API key
const url = 'https://go.getblock.io/YOUR-API-KEY/';

// Request payload
const payload = {
  jsonrpc: '2.0',
  method: 'eth_getTransactionByHash',
  params: [
    '0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c', // Transaction hash
  ],
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, payload, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Response:', response.data);
  })
  .catch((error) => {
    console.error('Error:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getTransactionByHash method and prints the returned transaction information. Make sure to replace \<ACCESS-TOKEN> with your actual API token. This script serves as an eth\_getTransactionByHash example for developers to understand the implementation in Python.

The Web3 eth\_getTransactionByHash method can also be used in Web3 libraries for Ethereum, providing an interface to access detailed transaction data based on a specific transaction hash.
