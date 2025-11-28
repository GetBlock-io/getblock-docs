---
description: >-
  Retrieve transaction information by block number and index using
  eth_getTransactionByBlockNumberAndIndex. Essential for tracking and validating
  transactions within specific blocks on the Ethereum.
---

# eth\_getTransactionByBlockNumberAndIndex - Ethereum

{% hint style="success" %}
This method returns transaction information for the specified block number and transaction index position. It is an essential tool for developers needing detailed transaction data from specific blocks.
{% endhint %}

The Ethereum eth\_getTransactionByBlockNumberAndIndex method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_getTransactionByBlockNumberAndIndex RPC Ethereum method is especially helpful for tracking and validating transactions within particular blocks.

### Supported Networks

The eth\_getTransactionByBlockNumberAndIndex RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

* QUANTITY | TAG: An integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.
* QUANTITY: An integer representing the transaction index position within the block.

### Request

URL

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
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": [
        "0x52A96E",
        "0x1"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["0x52A96E", "0x1"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the transaction details at the specified block number and index. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x7f0c47c49d70010028085c26f2fa9dfd7b6406a86fd522610f70852249632a81",
        "blockNumber": "0x52a96e",
        "chainId": "0x1",
        "from": "0xfbb1b73c4f0bda4f67dca266ce6ef42f520fbb98",
        "gas": "0x249f0",
        "gasPrice": "0xba43b7400",
        "hash": "0x0bf895891745037c4dce90f873b84cccd37396abcf32d113154b82fe16016b0d",
        "input": "0xa9059cbb0000000000000000000000009f050bc566289fe08f9534eb8b5b7437071a85ca000000000000000000000000000000000000000000000589b9c8aed550c82400",
        "nonce": "0x515f4c",
        "r": "0xe0ec15e49c7ef372fe7393dbb814bc69a007a89632f2921a540a6b975a2099cf",
        "s": "0x62f9e1d600011370d83c8ec2a04f0d4e406eb286120c8c0767ff176b3ac1789d",
        "to": "0xa74476443119a942de498590fe1f2454d7d4ac0d",
        "transactionIndex": "0x1",
        "type": "0x0",
        "v": "0x26",
        "value": "0x0"
    }
}
```

### Response Description

* beforeblockHash: The hash of the block containing the transaction.
* blockNumber: The number of the block containing the transaction.
* chainId: The chain ID of the Ethereum network.
* from: The address of the sender.
* gas: The gas provided by the sender.
* gasPrice: The gas price provided by the sender in Wei.
* hash: The hash of the transaction.
* input: The input data for the transaction.
* nonce: The number of transactions made by the sender prior to this one.
* r, s, v: Components of the transaction signature.
* to: The address of the receiver.
* transactionIndex: The index position of the transaction within the block.
* type: The transaction type.
* value: The value transferred in Wei.

### Use Case

The eth\_getTransactionByBlockNumberAndIndex method is useful for tracking and verifying transactions within specific blocks. By using the block number and transaction index, developers can easily access detailed information about individual transactions. In case of an eth\_getTransactionByBlockNumberAndIndex error, developers should verify that the provided block number and index are correct and correspond to an existing transaction within the block. An eth\_getTransactionByBlockNumberAndIndex example can help developers understand the correct usage of this method.

### Code Example

You can also make requests to the eth\_getTransactionByBlockNumberAndIndex method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getTransactionByBlockNumberAndIndex",
    "params": [
        "0x52A96E",
        "0x1"
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
  method: 'eth_getTransactionByBlockNumberAndIndex',
  params: [
    '0x52A96E', // Block number
    '0x1',      // Transaction index
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

This Python script sends a request to the eth\_getTransactionByBlockNumberAndIndex method and prints the returned transaction information. Make sure to replace \<ACCESS-TOKEN> with your actual API token. This script serves as an eth\_getTransactionByBlockNumberAndIndex example for developers to understand the implementation in Python.

The Web3 eth\_getTransactionByBlockNumberAndIndex method can also be used in Web3 libraries for Ethereum, providing an interface to retrieve detailed transaction data by referencing a specific block and index.
