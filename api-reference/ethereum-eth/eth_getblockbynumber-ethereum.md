---
description: >-
  Retrieve detailed information about a specific Ethereum block by its block
  number using eth_getBlockByNumber. Essential for interacting with the Ethereum
  blockchain via JSON-RPC.
---

# eth\_getBlockByNumber - Ethereum

{% hint style="success" %}
The eth\_getBlockByNumber method is a part of the eth\_getBlockByNumber RPC Ethereum protocol and allows users to retrieve detailed information about a specific Ethereum block, identified by its block number.
{% endhint %}

This method works by connecting to the Ethereum network via JSON-RPC, a remote procedure call (RPC) protocol used for communication between clients and servers. Core API endpoints provide the functionality to interact with the blockchain, and eth\_getBlockByNumber is one of the most commonly used methods within these endpoints.

### Supported Networks

The eth\_getBlockByNumber RPC Ethereum method works on the following Ethereum network types

* **Mainnet**
* **Testnet**: Sepolia, Hoodi

### Parameters

1. QUANTITY | TAG (required): An integer representing a block number or one of the following string tags:
   * latest: The most recent block.
   * earliest: The first block of the blockchain.
   * pending: The block currently being mined.

Boolean (required): If set to true, the method returns full transaction objects with complete details. If set to false, only the transaction hashes are returned.

### Request

URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To request a block by its number using the eth\_getBlockByNumber method, the following JSON object is sent in a POST request via the Ethereum Web3 API

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": [
        "0x68B3",
        true
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
"method": "eth_getBlockByNumber",
"params": ["0x68B3", true],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server will respond with a JSON object containing block information. An example response when full transaction objects are requested looks like this

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x1046bb7e3f8",
        "extraData": "0x476574682f76312e302e302d30636463373634372f6c696e75782f676f312e34",
        "gasLimit": "0x1388",
        "gasUsed": "0x0",
        "hash": "0xf7756d836b6716aaeffc2139c032752ba5acf02fe94acb65743f0d177554b2e2",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0x33bc13fdf135073277971b4d9f4f72082e907996",
        "mixHash": "0x8c2dc0f970fa3aa6beb64c9f06a202a4314acfa4effaa4c75fd5bc9f9c77a519",
        "nonce": "0x28df43dd283aab1d",
        "number": "0x68b3",
        "parentHash": "0xbc33aa8829350cc2e3ba7cf64d4beb2f1b554d570efc8bccb7b05ef50d76a47a",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x223",
        "stateRoot": "0x8af5429b649f9fc633ce3c95219026fd08a249867e28c7eab22994eaa6125bb9",
        "timestamp": "0x55bf47e3",
        "totalDifficulty": "0x3f3cfd84833af0",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}
```

### Response Description

The response includes various details about the block:

* difficulty: The mining difficulty of the block.
* extraData: Additional data related to the block.
* gasLimit: The maximum amount of gas allowed for the block.
* gasUsed: The total gas used by all transactions in the block.
* hash: The block's hash.
* logsBloom: A bloom filter for the logs of the block.
* miner: The address of the miner who mined the block.
* mixHash: The hash used for the mining process.
* nonce: The proof-of-work nonce.
* number: The block number.
* parentHash: The hash of the previous block.
* receiptsRoot: The root hash of the receipts.
* sha3Uncles: A hash representing the uncles (if any).
* size: The size of the block.
* stateRoot: The root hash of the state trie.
* timestamp: The block's timestamp.
* totalDifficulty: The cumulative difficulty of the chain up to and including the block.
* transactions: A list of transaction objects (if true was passed for full transaction details).
* uncles: A list of uncles associated with the block.

### Use Case

The eth\_getBlockByNumber method is primarily used to query a block in the Ethereum blockchain by its block number or one of the predefined tags such as latest or pending. This can be useful for blockchain explorers, wallet apps, or anyone who needs to retrieve detailed block data, including transaction information or general block metrics. It’s part of the Ethereum API that allows developers to interact with the blockchain programmatically.

### Code Example

Below is an example Python code snippet to make a request to the eth\_getBlockByNumber method using the requests library

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
    "method": "eth_getBlockByNumber",
    "params": [
        "0x68B3",
        True
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

// Define the API URL and access token
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_getBlockByNumber',
  params: [
    '0x68B3',
    true
  ],
  id: 'getblock.io'
};

// Send the POST request
axios.post(url, data, { headers })
  .then(response => {
    // Print the result
    console.log(JSON.stringify(response.data, null, 4));
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

### Common Errors

Invalid Block Number or Tag: If an invalid block number or tag (e.g., latest, earliest, or pending) is provided, the API will return an error.

* Error Message: "eth\_getBlockByNumber error": "Invalid block number or tag"

Transaction Data Retrieval Failure: If the requested block does not contain transaction data, an empty array will be returned for the transactions field.

* Error Message: "eth\_getBlockByNumber error": "No transaction data available"

API Rate Limiting: If too many requests are made in a short period, you may encounter rate-limiting errors.

Error Message: "eth\_getBlockByNumber error": "Rate limit exceeded"
