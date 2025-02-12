---
description: >-
  The eth_getUncleByBlockNumberAndIndex method retrieves an uncle block by its
  block number and index, helping developers analyze network performance and
  consensus behavior using Ethereum's API
---

# eth\_getUncleByBlockNumberAndIndex - Ethereum

{% hint style="success" %}
This method returns an uncle block specified by the block number and index
{% endhint %}

The eth\_getUncleByBlockNumberAndIndex method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. This method returns an uncle block specified by the block number and index. The eth\_getUncleByBlockNumberAndIndex RPC Ethereum method helps developers to retrieve information about uncle blocks, which can be useful for analyzing network performance and consensus behavior.

### Supported Networks

The eth\_getUncleByBlockNumberAndIndex RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* QUANTITY | TAG: The block number, or one of the string tags latest, earliest, or pending.
* QUANTITY: The index of the uncle within the block.
* value: Placeholder for any additional data.

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
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": [
        "0x7689D2",
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
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": [
        "0x7689D2",
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
        "difficulty": "0x77daec467bf93",
        "extraData": "0x50505945206e616e6f706f6f6c2e6f7267",
        "gasLimit": "0x7a121d",
        "gasUsed": "0x7a0f7b",
        "hash": "0x42d83ae9c0743f4b1f9c61ff7ea8b164c1bab3627decd49233760680be006ecf",
        "logsBloom": "0x888200800000340120220008640200500408006100038400100581c000080240080a0014e8002010080004088040004022402a000c18010001400100002a041141a0610a0052900600041018c0002a0003090020404c00206010010513d00020005380124e08050480710000000108401012b0901c1424006000083a10a8c1040100a0440081050210124400040044304070004001100000012600806008061d0320800000b40042160600002480000000800000c0002100200940801c000820800048024904710000400640490026000a44300309000286088010c2300060003011380006400200812009144042204810209020410a84000410520c08802941",
        "miner": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5",
        "mixHash": "0xf977fcdb52868be410b75ef2becc35cc312f13ab0a6ce400ecd9d445f66fa3f2",
        "nonce": "0x628b28403bf1e3d3",
        "number": "0x7689d0",
        "parentHash": "0xb32cfdfbf4adb05d30f02fcc6fe039cc6666402142954051c1a1cb9cc91aa11e",
        "receiptsRoot": "0x9c7c8361d1a24ea2841432234c81974a9920d3eba2b2b1c496b5f925a95cb4ac",
        "sha3Uncles": "0x7d972aa1b182b7e93f1db043f03fbdbfac6874fe7e67e162141bcc0aefa6336b",
        "size": "0x21a",
        "stateRoot": "0x74e97b77813146344d75acb5a52a006cc6dfaca678a10fb8a484a8443e919272",
        "timestamp": "0x5cdcc0a7",
        "transactionsRoot": "0x1d21626afddf05e5866de66ca3fcd98f1caf5357eba0cc6ec675606e116a891b",
        "uncles": [],
    }
}

```

### Response Description

* value: A placeholder for any additional value information that might be associated with the uncle block.
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

The eth\_getUncleByBlockNumberAndIndex method is useful for developers who need to analyze the structure of uncle blocks in Ethereum. This method helps to gain insights into the consensus mechanism and understand the process of including uncles in the blockchain. By using the eth\_getUncleByBlockNumberAndIndex RPC Ethereum method, developers can easily access all necessary details about an uncle block. In case of an eth\_getUncleByBlockNumberAndIndex error, developers should verify that the provided block number and index are correct. An eth\_getUncleByBlockNumberAndIndex example can illustrate the correct usage of this method.

### Code Example

You can also make requests to the eth\_getUncleByBlockNumberAndIndex method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getUncleByBlockNumberAndIndex",
    "params": [
        "0x7689D2",
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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = {
  'Content-Type': 'application/json'
};

const data = {
  jsonrpc: '2.0',
  method: 'eth_getUncleByBlockNumberAndIndex',
  params: [
    '0x7689D2',
    '0x0'
  ],
  id: 'getblock.io'
};

// Отправка POST-запроса с использованием axios
axios.post(url, data, { headers })
  .then(response => {
    console.log('Ответ:', response.data);
  })
  .catch(error => {
    console.error('Ошибка:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getUncleByBlockNumberAndIndex method and prints the returned uncle block information. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The eth\_getUncleByBlockNumberAndIndex method can also be used in Web3 libraries for Ethereum, providing an interface to access uncle block data for various use cases, including network analysis and blockchain research.

The Web3 eth\_getUncleByBlockNumberAndIndex method is also available in Web3 libraries, allowing developers to interact with uncle block data programmatically. The Ethereum eth\_getUncleByBlockNumberAndIndex method is an important tool for understanding the structure of uncle blocks, making it a valuable feature of the Ethereum JSON RPC API and Core API Endpoints.
