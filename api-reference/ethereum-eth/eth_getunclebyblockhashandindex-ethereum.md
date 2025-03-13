---
description: >-
  Access Ethereum block, transaction, and event data via JSON-RPC methods.Easily
  retrieve block details, transaction information, and gas prices for efficient
  dApp development and blockchain interaction
---

# eth\_getUncleByBlockHashAndIndex - Ethereum

{% hint style="success" %}
This method returns an uncle block specified by the block hash and index.
{% endhint %}

The eth\_getUncleByBlockHashAndIndex method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_getUncleByBlockHashAndIndexRPC Ethereum method allows developers to retrieve detailed information about uncle blocks, which can be useful for understanding network activity and consensus. The value of the uncle block's information can be crucial for various analysis purposes.

### Supported Networks

The eth\_getUncleByBlockHashAndIndexRPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 32-byte block hash.
* QUANTITY: The index of the uncle.

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
    "method": "eth_getUncleByBlockHashAndIndex",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x0"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{
    "jsonrpc": "2.0",
    "method": "eth_getUncleByBlockHashAndIndex",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x0"
    ],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the uncle block details. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x76b123df93230",
        "extraData": "0x50505945206e616e6f706f6f6c2e6f7267",
        "gasLimit": "0x7a121d",
        "gasUsed": "0x7a0175",
        "hash": "0xc20189c0b1a4a23116ab3b177e929137f6e826f17fc4c2e880e7258c620e9817",
        "logsBloom": "0x890086c024487ca422be846a201a10e41bc2882902312116c1119609482031e9c000e2a708004a10281024028020c505727a12570c4810121c59024490b040894406a1c23c37a0094810921da3923600c71c03044b40924280038d07ab91964a008084264a01641380798840805a284cce201a8026045451002500113a00de441001320805ca2840037000111640d090442c11116d2112948084240242340400236ce81502063401dcc214b9105194d050884721c1208800b20501a4201400276004142f118e60808284506979a86e050820101c170c185e2310005205a82a2100382422104182090184800c02489e033440218142140045801c024cc1818485",
        "miner": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5",
        "mixHash": "0xf557cc827e058862aa3ea1bd6088fb8766f70c0eac4117c56cf85b7911f82a14",
        "nonce": "0xd320b48904347cdd",
        "number": "0x768964",
        "parentHash": "0x98d752708b3677df8f439c4529f999b94663d5494dbfc08909656db3c90f6255",
        "receiptsRoot": "0x0f838f0ceb73368e7fc8d713a7761e5be31e3b4beafe1a6875a7f275f82da45b",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x21a",
        "stateRoot": "0xa0c7d4fca79810c89c517eff8dadb9c6d6f4bcc27c2edfb301301e1cf7dec642",
        "timestamp": "0x5cdcbba6",
        "transactionsRoot": "0x866e38e91d01ef0387b8e07ccf35cd910224271ccf2b7477b8c8439e8b70f365",
        "uncles": []
    }
}

```

### Response Description

* difficulty: The difficulty of the uncle block.
* extraData: Any extra data included in the uncle block.
* gasLimit: The gas limit of the uncle block.
* gasUsed: The gas used by the uncle block.
* hash: The hash of the uncle block.
* logsBloom: Bloom filter for light clients to quickly retrieve related logs.
* miner: The address of the miner that mined the uncle block.
* mixHash: A hash used in the mining process.
* nonce: The hash that proves the work done by the miner.
* number: The block number of the uncle.
* parentHash: The hash of the parent block.
* receiptsRoot: The root of the receipts trie of the uncle block.
* sha3Uncles: The SHA3 hash of the uncles data in the block.
* size: The size of the uncle block in bytes.
* stateRoot: The root of the state trie.
* timestamp: The timestamp of when the uncle was mined.
* transactionsRoot: The root of the transaction trie of the uncle block.
* uncles: Array of uncle hashes.

### Use Case

The response to this method can also include important information for analyzing specific transaction that might be related to the uncle blocks.The eth\_getUncleByBlockHashAndIndex method is useful for developers who need to analyze the structure of uncle blocks in Ethereum. This method helps to gain insights into the consensus mechanism and understand the process of including uncles in the blockchain. By using the eth\_getUncleByBlockHashAndIndexRPC Ethereum method, developers can easily access all necessary details about an uncle block. In case of an eth\_getUncleByBlockHashAndIndex error, developers should verify that the provided block hash and index are correct. An eth\_getUncleByBlockHashAndIndex example can illustrate the correct usage of this method.

### Code Example

You can also make requests to the eth\_getUncleByBlockHashAndIndex method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getUncleByBlockHashAndIndex",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x0"
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
    '0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c' // Transaction hash
  ],
  id: 'getblock.io'
};

// Axios POST request
axios
  .post(url, payload, {
    headers: {
      'Content-Type': 'application/json'
    }
  })
  .then((response) => {
    console.log('Transaction Receipt:', response.data.result);
  })
  .catch((error) => {
    console.error('Error:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the **eth\_getUncleByBlockHashAndIndex** method and prints the returned uncle block information. Make sure to replace \<ACCESS-TOKEN> with your actual API token.&#x20;

The eth\_getUncleByBlockHashAndIndex method can also be used in Web3 libraries for Ethereum, providing an interface to access uncle block data for various use cases, including network analysis and blockchain research.

The Ethereum eth\_getUncleByBlockHashAndIndex method is an important tool for understanding the structure of uncle blocks, making it a valuable feature of the Ethereum JSON RPC API and Core API Endpoints.
