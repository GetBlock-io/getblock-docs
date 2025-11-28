---
description: >-
  Retrieve detailed or basic block information using a block hash. Fetch full
  block data or key details like transaction hashes, ideal for blockchain
  analysis or lightweight data retrieval.
---

# eth\_getBlockByHash - Ethereum

{% hint style="success" %}
The eth\_getBlockByHash method is part of the Core API in the Ethereum JSON-RPC retrieves information about a specific block using its block hash.
{% endhint %}

This method allows users to fetch either the full block data, including detailed transaction details, or just the basic block information with transaction hashes. This flexibility is beneficial for applications that require either comprehensive blockchain analysis or a lighter response focused on key block data.

### Supported Networks

The eth\_getBlockByHash method supports the following Ethereum networks

* **Mainnet**
* **Testnet**: Sepolia, Hoodi

### Parameters

1. DATA, string (32-byte hash): The hash of the block to retrieve (required).
2. Boolean, true | false:

* true: Returns full transaction objects with complete transaction details.
* false: Returns only the transaction hashes.

### Request

URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To request the eth\_getBlockByHash method, you can use the following curl command. The request sends a JSON-RPC query to the Ethereum network via the GetBlock API

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        false
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
"method": "eth_getBlockByHash",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7", false],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the block information. Below is an example response when full transaction objects are requested

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x76bffa240f156",
        "extraData": "0xd883010817846765746888676f312e31302e31856c696e7578",
        "gasLimit": "0x7a1200",
        "gasUsed": "0x79c0a3",
        "hash": "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "logsBloom": "0x124334a4406810389873106b900344dc2c91908c0898000c8208a12040ca010080080d0100a000100602226387402285b6e016522e48490301d8e1a820ba0902e01493459022b0216884f24c80814111820c200443c0a05804048fc4c1428010000881001a810224c00ad0a4200c380204eabad0002544bb11f141714048020484a24100431689420814000481404c900c30116101512050010a7092020233700600ec404216200199c000604048be630151141481160001404213901c20211180147202898b0814081b469014202c088d312088010e211c200a82c2018ce0a120116626481b0a0d0d8280804944a9853184808a801208510498140e21906105",
        "miner": "0x09ab1303d3ccaf5f018cd511146b07a240c70294",
        "mixHash": "0xbf802f1c1aff805344df3d73ae005b1c21fa2e3475091c67a114a6585663c50b",
        "nonce": "0x0a6ac00001626799",
        "number": "0x768965",
        "parentHash": "0xbacb05fe267e4c7a8699c34dafe45af7d60253048ed1bdb00db3c66481c341b0",
        "receiptsRoot": "0x48ddd33fa5aa32e9fef8486b19997902da5c6f39bb6088190f6d6af3aa47739f",
        "sha3Uncles": "0x654afae449b771cef795bd94d8fb27f5f0064bb076eee651a2e434322cd94695",
        "size": "0x74e2",
        "stateRoot": "0x3e59f341a06d4a2513ff498ac40fa435f040d645be1c1ce02e71b683be0b2c53",
        "timestamp": "0x5cdcbbad",
        "totalDifficulty": "0x229ad3b36bcee81c393",
        "transactions": [
            "0x6bd9f57cde1c1518fff47ebbd9c505d06eeb4221b7c861a772f66c5970a358ad",
            "0xf0c969e381ccd3db03856991c83320b7f65b541ef1dbd1687bf6a66f0e8f68c2",
            "0xd8d81b0362c607493e737667a392182f9074f60f11d2df66c0b5d3ce00e0186b",
            "0xa96813cb6331b3fe4318c700aaf887d0d5d3bedd4b1b32e1dd0428e29934016c"
        ],
        "transactionsRoot": "0x16998be3d0694d80ceb1ccf5474929383357f462fb1ce005988ce425c3321f2e",
        "uncles": [
            "0xc20189c0b1a4a23116ab3b177e929137f6e826f17fc4c2e880e7258c620e9817"
        ]
    }
}
```

### Response Description

* **id** (`string`): The identifier of the request, as sent by the client. In this case, `"getblock.io"`.
* **jsonrpc** (`string`): The version of JSON-RPC, here `"2.0"`.
* **result** (`object`): An object containing information about the block, with the following fields:
  * **difficulty** (`string`): The current difficulty of the block, in hexadecimal format.
  * **extraData** (`string`): Additional data related to the block, in hexadecimal format.
  * **gasLimit** (`string`): The gas limit for the block, in hexadecimal format.
  * **gasUsed** (`string`): The amount of gas used in the block, in hexadecimal format.
  * **hash** (`string`): The hash of the current block, in hexadecimal format.
  * **logsBloom** (`string`): The logs bloom filter for transactions in the block, in hexadecimal format.
  * **miner** (`string`): The address of the miner who mined the block.
  * **mixHash** (`string`): The hash used in the mining process of the block, in hexadecimal format.
  * **nonce** (`string`): A random value used by the miner to find the block hash.
  * **number** (`string`): The block number, in hexadecimal format.
  * **parentHash** (`string`): The hash of the parent block, in hexadecimal format.
  * **receiptsRoot** (`string`): The root hash of all receipts of transactions in the block, in hexadecimal format.
  * **sha3Uncles** (`string`): The hash of the list of all uncle blocks in the block, in hexadecimal format.
  * **size** (`string`): The size of the block in bytes, in hexadecimal format.
  * **stateRoot** (`string`): The root hash of the state after processing the block, in hexadecimal format.
  * **timestamp** (`string`): The timestamp of the block, in hexadecimal format.
  * **totalDifficulty** (`string`): The total difficulty up to and including the current block, in hexadecimal format.
  * **transactions** (`array of strings`): A list of transaction hashes included in the block.
  * **transactionsRoot** (`string`): The root hash of all transactions in the block, in hexadecimal format.
  * **uncles** (`array of strings`): A list of uncle block hashes associated with this block.

### Use Case

For developers and blockchain explorers, the eth\_getBlockByHash method is crucial for obtaining detailed information about specific blocks in the Ethereum blockchain. This method can be used for:

* Verifying transactions within a particular block using the transaction hash.
* Retrieving comprehensive block data for in-depth blockchain analysis.
* Debugging and investigating specific events on the Ethereum network by examining block data.

### Code Example

You can also make requests to the eth\_getBlockByHash method programmatically using Python. Below is an example using the requests library

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
    "method": "eth_getBlockByHash",
    "params": [
        "0xb3b20624c0870029c179a9610d55802dc8b651fdfebedb1653d1c06e165faf22",
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

const accessToken = '<ACCESS-TOKEN>';
const url = `https://go.getblock.io/${accessToken}/`;

const data = {
  jsonrpc: '2.0',
  method: 'eth_getBlockByHash',
  params: [
    '0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7',
    false
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

This Python script sends a request to the eth\_getBlockByHash method and prints the returned block information. Make sure to replace with your actual API token. This method provides access to both Ethereum Mainnet and test networks like Sepolia and Goerli, enabling comprehensive blockchain analysis across multiple environments.
