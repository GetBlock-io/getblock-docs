---
description: >-
  Retrieve transaction receipt details using eth_getTransactionReceipt.
  Essential for obtaining information about transaction execution, including gas
  usage, status, and logs on the Ethereum blockchain.
---

# eth\_getTransactionReceipt - Ethereum

{% hint style="success" %}
This method returns the receipt of a transaction by its transaction hash.
{% endhint %}

The eth\_getTransactionReceipt method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. Receipts for pending transactions are not available. If revert reasons are enabled, the receipt includes available revert reasons in the response. The eth\_getTransactionReceipt RPC Ethereum method is essential for obtaining details about transaction execution, such as gas usage, status, and logs.

### Supported Networks

The eth\_getTransactionReceipt RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 32-byte hash of the transaction.

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
    "method": "eth_getTransactionReceipt",
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
"method": "eth_getTransactionReceipt",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the transaction receipt details. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
        "blockNumber": "0x107d7b0",
        "contractAddress": null,
        "cumulativeGasUsed": "0x19aac9a",
        "effectiveGasPrice": "0xb9029a7ea",
        "from": "0x901c7c311d39e0b26257219765e71e8db3107a81",
        "gasUsed": "0x27fb4",
        "logs": [
            {
                "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
                "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
                "blockNumber": "0x107d7b0",
                "data": "0x0000000000000000000000000000000000000000000000000000000103f1bfef",
                "logIndex": "0x24e",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a82f91562e1cef9dec93a4ad328d01ea7827910a",
                    "0x000000000000000000000000901c7c311d39e0b26257219765e71e8db3107a81"
                ],
                "transactionHash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
                "transactionIndex": "0xfc"
            },
            {
                "address": "0xb7135877cd5d40aa3b086ac6f21c51bbafbbb41f",
                "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
                "blockNumber": "0x107d7b0",
                "data": "0x00000000000000000000000000000000000000000003f6526b99745385c00000",
                "logIndex": "0x24f",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x000000000000000000000000901c7c311d39e0b26257219765e71e8db3107a81",
                    "0x000000000000000000000000000000000022d473030f116ddee9f6b43ac78ba3"
                ],
                "transactionHash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
                "transactionIndex": "0xfc"
            },
            {
                "address": "0xb7135877cd5d40aa3b086ac6f21c51bbafbbb41f",
                "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
                "blockNumber": "0x107d7b0",
                "data": "0x000000000000000000000000000000000000000000000a968163f0a57b400000",
                "logIndex": "0x250",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000901c7c311d39e0b26257219765e71e8db3107a81",
                    "0x000000000000000000000000a82f91562e1cef9dec93a4ad328d01ea7827910a"
                ],
                "transactionHash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
                "transactionIndex": "0xfc"
            },
            {
                "address": "0xa82f91562e1cef9dec93a4ad328d01ea7827910a",
                "blockHash": "0xe6262c1924326d12b88aaa35a95a0c7cdd11f2d20ebae84618484120bd037c34",
                "blockNumber": "0x107d7b0",
                "data": "0x000000000000000000000000000000000000000000000a968163f0a57b400000fffffffffffffffffffffffffffffffffffffffffffffffffffffffefc0e40110000000000000000000000000000000000000000000004f77993b72687d0b8d40000000000000000000000000000000000000000000000002672ab0fa51842cafffffffffffffffffffffffffffffffffffffffffffffffffffffffffffb6981",
                "logIndex": "0x251",
                "removed": false,
                "topics": [
                    "0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67",
                    "0x000000000000000000000000ef1c6e67703c7bd7107eed8303fbe6ec2554bf6b",
                    "0x000000000000000000000000901c7c311d39e0b26257219765e71e8db3107a81"
                ],
                "transactionHash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
                "transactionIndex": "0xfc"
            }
        ],
        "logsBloom": "0x00000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000010000000000800020000000000000200000000040000000000800104008000000000000000000000800000000001200000000000000000000000000000000000000000000000020000000020010000800000000000000000000000000000000000000000000000000000000000000120000020800000000000000100084000000000000000000000000000000000000000000000002000000000000001000000000400000000000000000000000000000000010000000000000000000000000000000000000000000000000000090000000",
        "status": "0x1",
        "to": "0xef1c6e67703c7bd7107eed8303fbe6ec2554bf6b",
        "transactionHash": "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
        "transactionIndex": "0xfc",
        "type": "0x2"
    }
}
```

### Response Description

* blockHash: The hash of the block containing the transaction.
* blockNumber: The number of the block containing the transaction.
* contractAddress: The contract address created, if the transaction was a contract creation, otherwise null.
* cumulativeGasUsed: The total amount of gas used when this transaction was executed in the block.
* effectiveGasPrice: The actual price per unit of gas paid.
* from: The address of the sender.
* gasUsed: The amount of gas used by this specific transaction alone.
* logs: An array of log objects generated by this transaction.
* logsBloom: Bloom filter for light clients to quickly retrieve related logs.
* status: The status of the transaction (0x1 indicates success, 0x0 indicates failure).
* to: The address of the receiver.
* transactionHash: The hash of the transaction.
* transactionIndex: The index position of the transaction within the block.
* type: The transaction type.
* value: The value transferred in Wei.

### Use Case

The eth\_getTransactionReceipt method is particularly useful for developers who need detailed information about a transaction, such as its status, gas usage, and generated logs. This is crucial for debugging, auditing, and understanding the effects of contract executions. By using the eth\_getTransactionReceipt RPC Ethereum method, developers can retrieve all necessary details about a transaction's execution. In case of an eth\_getTransactionReceipt error, developers should verify that the provided transaction hash is correct and corresponds to an existing transaction. An eth\_getTransactionReceipt example can help illustrate how to properly use this method.

### Code Example

You can also make requests to the eth\_getTransactionReceipt method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getTransactionReceipt",
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

// Replace <ACCESS_TOKEN> with your actual Access Token
const url = 'https://go.getblock.io/<ACCESS_TOKEN>/';

// Request payload
const payload = {
  jsonrpc: '2.0',
  method: 'eth_getTransactionReceipt',
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
    console.log('Transaction Receipt:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching transaction receipt:', error.message);
  });

```
{% endtab %}
{% endtabs %}

This Python script sends a request to the **eth\_getTransactionReceipt** method and prints the returned transaction receipt information. Make sure to replace \<ACCESS-TOKEN> with your actual API token.&#x20;

The eth\_getTransactionReceipt method can also be used in Web3 libraries for Ethereum, providing an interface to access transaction receipt data for various use cases, including auditing, debugging, and verifying contract behavior.

The Ethereum eth\_getTransactionReceipt method provides detailed insights into transaction execution, making it an essential tool for developers working with Ethereum's JSON RPC API and Core API Endpoints.
