---
description: >-
  Retrieve the number of transactions in a specific Ethereum block by its block
  hash using eth_getBlockTransactionCountByHash.Essential for tracking
  transaction data and building blockchain explorers.
---

# eth\_getBlockTransactionCountByHash - Ethereum

{% hint style="success" %}
This method allows developers to retrieve the number of transactions that are included in a specific block, identified by its block hash
{% endhint %}

The eth\_getBlockTransactionCountByHash method is part of the Ethereum Core API and works by connecting to the Ethereum network via the JSON RPC protocol. The eth\_getBlockTransactionCountByHash RPC Ethereum method is a critical tool for applications that need to monitor the number of transactions in a given Ethereum block or build advanced blockchain explorers that track transaction data in a more granular way.

### Supported Networks

The eth\_getBlockTransactionCountByHash RPC Ethereum method works on the following Ethereum network types:

* **Mainnet**
* **Testnet**: Sepolia, Holesky

These networks allow you to request the number of transactions within a block, given its block hash, on both the main Ethereum network and various test networks.

### Parameters

This method takes the following parameter:

* data - None\
  A 32-byte block hash that uniquely identifies the block for which the transaction count is to be retrieved.

### Request

URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To invoke the eth\_getBlockTransactionCountByHash method, use the following request format

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
    ],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="wss" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByHash",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

This request sends the block hash to the server, asking it to return the number of transactions for the block with the corresponding hash.

### Response

The response from the eth\_getBlockTransactionCountByHash method will look like this

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x85"
}
```

In this example, the block identified by the provided hash contains 0x85 (133 in decimal) transactions.

### Response Description

The eth\_getBlockTransactionCountByHash method returns the value of the transaction count in hexadecimal format. This response includes the following fields:

* id: The unique identifier for the request.
* jsonrpc: The JSON RPC version used.
* result: The transaction count for the block, represented as a hexadecimal string.

For instance, a response of "result": "0x85" indicates that there are 133 transactions in the block corresponding to the given block hash.

### Use Case

The eth\_getBlockTransactionCountByHash method is useful when you need to retrieve the number of transactions in a specific Ethereum block by providing its hash. This is often used in blockchain explorers, transaction analyzers, and applications that track the number of transactions in each block for indexing or statistical purposes. By querying this method, developers can easily retrieve block-level transaction data without needing to fetch the entire block's transaction details.

### Code Example

Here’s how you can use the Web3 eth\_getBlockTransactionCountByHash method in a JavaScript environment to retrieve the transaction count of a block by its hash:

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
    "method": "eth_getBlockTransactionCountByHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
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

const accessToken = '<ACCESS-TOKEN>';
const url = `https://go.getblock.io/${accessToken}/`;

const data = {
  jsonrpc: '2.0',
  method: 'eth_getBlockTransactionCountByHash',
  params: [
    '0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7'
  ],
  id: 'getblock.io'
};

axios.post(url, data, {
  headers: {
    'Content-Type': 'application/json'
  }
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

This Web3 eth\_getBlockTransactionCountByHash example demonstrates how to retrieve the transaction count for a given block hash. It can be used in a variety of decentralized applications (dApps) that need to interact with the Ethereum blockchain.

### Common Errors

1. eth\_getBlockTransactionCountByHash error - This error may occur if the provided block hash is invalid or if the block does not exist in the network.
2. Invalid block hash - If the block hash is improperly formatted or incorrect, the Ethereum network will not be able to find the corresponding block.
3. Network issues - If there are issues with connecting to the Core API or the JSON RPC server, you may receive a timeout or connectivity error.
