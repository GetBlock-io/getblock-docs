---
description: >-
  Retrieve transaction information by block hash and index using
  eth_getTransactionByBlockHashAndIndex. Essential for tracking transactions
  within specific blocks on the Ethereum blockchain.
---

# eth\_getTransactionByBlockHashAndIndex - Ethereum

{% hint style="success" %}
This method returns transaction information for the specified block hash and transaction index position, making it an essential tool for developers needing detailed data about transactions within specific blocks
{% endhint %}

Ethereum eth\_getTransactionByBlockHashAndIndex JSON-RPC Core API, used to interact with Ethereum nodes. The eth\_getTransactionByBlockHashAndIndex RPC Ethereum method helps in tracking transactions based on their order within a block.

### Supported Networks

The eth\_getTransactionByBlockHashAndIndex RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 32-byte hash of the block for which to retrieve the transaction.
* QUANTITY: An integer representing the transaction index position within the block.

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
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x2"
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
"method": "eth_getTransactionByBlockHashAndIndex",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7", "0x2"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the transaction details at the specified block hash and index. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "blockNumber": "0x768965",
        "chainId": "0x1",
        "from": "0xb38fd09d9735a0a41bd90a12e01eaf9c2962338f",
        "gas": "0x30d40",
        "gasPrice": "0xdf8475800",
        "hash": "0xd8d81b0362c607493e737667a392182f9074f60f11d2df66c0b5d3ce00e0186b",
        "input": "0x3912521500000000000000000000000085e323e41cfff4ea8b7668f8c591dcbc6d40185e000000000000000000000000000000000000000000000000a7c3416a6470dc0000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000005cdcbce0000000000000000000000000000000000000000000000000000000000000496800000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004115835c239bcae9655b584463af1ec5b4920eed04edfeedef030a9e92ef5ef8b83032f4a209c00ea627ba6e0129f158085833348cc6f4987313f8f0eea33013391b00000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x495f",
        "r": "0x69d47969d34be198f9896dc979554093833e70c9f672410a3f7297386e2d7d91",
        "s": "0x1d3e4cb5a92a1ec5694df501d6199da69e21c426e015e52a5bb848995712d58a",
        "to": "0x560e389a2b032319e742a59ae8bafa62671089fe",
        "transactionIndex": "0x2",
        "type": "0x0",
        "v": "0x25",
        "value": "0x0"
    }
}
```

### Response Description

* **id**: The identifier of the request sent by the client. In this case, it is `"getblock.io"`.
* **jsonrpc**: The version of the JSON-RPC protocol, used to ensure compatibility. The value is `"2.0"`.
* **blockHash**: The hash of the block that contains this transaction.
* **blockNumber**: The number of the block that contains this transaction, in hexadecimal format.
* **chainId**: The ID of the chain on which the transaction was executed, in hexadecimal format.
* **from**: The address of the sender of the transaction.
* **gas**: The amount of gas provided by the sender, in hexadecimal format.
* **gasPrice**: The price of each unit of gas, in hexadecimal format.
* **hash**: The hash of the transaction.
* **input**: The data sent along with the transaction.
* **nonce**: The number of transactions sent from the sender's address before this one, in hexadecimal format.
* **r**: The first part of the cryptographic signature for the transaction.
* **s**: The second part of the cryptographic signature for the transaction.
* **to**: The address of the recipient of the transaction.
* **transactionIndex**: The index of the transaction within the block, in hexadecimal format.
* **type**: The type of the transaction, in hexadecimal format.
* **v**: The recovery id of the cryptographic signature for the transaction.
* **value**: The value transferred in the transaction, in Wei, in hexadecimal format.

### Use Case

The eth\_getTransactionByBlockHashAndIndex method is useful for tracking and verifying transactions within specific blocks. By using the block hash and transaction index, developers can easily access detailed information about individual transactions. In case of an eth\_getTransactionByBlockHashAndIndex error, developers should verify that the provided block hash and index are correct and correspond to an existing transaction within the block. An eth\_getTransactionByBlockHashAndIndex example can help developers understand the correct usage of this method.

### Code Example

You can also make requests to the eth\_getTransactionByBlockHashAndIndex method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x2"
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

// Replace <ACCESS-TOKEN> with your actual API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getTransactionByBlockHashAndIndex',
  params: [
    '0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7', // Block hash
    '0x2', // Transaction index
  ],
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, requestBody, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Transaction Details:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching transaction by block hash and index:', error.message);
  });

```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getTransactionByBlockHashAndIndex method and prints the returned transaction information. Make sure to replace \<ACCESS-TOKEN> with your actual API token. This script serves as an eth\_getTransactionByBlockHashAndIndex example for developers to understand the implementation in Python.

The Web3 eth\_getTransactionByBlockHashAndIndex method can also be used in Web3 libraries for Ethereum, providing an interface to retrieve detailed transaction data by referencing a specific block and index.
