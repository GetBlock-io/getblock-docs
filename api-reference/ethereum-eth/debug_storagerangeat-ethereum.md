---
description: >-
  The debug_storageRangeAt method is part of the Ethereum JSON RPC Core API,
  used for debugging purposes. It allows developers to retrieve contract storage
  data within a specified range.
---

# debug\_storageRangeAt - Ethereum

{% hint style="success" %}
Remix uses debug\_storageRangeAt to implement debugging. Use the Debuggertab in Remix instead of calling debug\_storageRangeAt directly.Returns the contract storage for the specified range.
{% endhint %}

The debug\_storageRangeAt method is part of the Ethereum JSON RPC Core API, designed for debugging purposes. This method allows developers to retrieve contract storage data within a specified range. While primarily used by tools like Remix, developers can call it directly to inspect and analyze on-chain storage data.

### Supported Networks

The debug\_storageRangeAt RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

The debug\_storageRangeAt method accepts the following parameters:

* blockHash: (data) The block hash to fetch storage from.
* txIndex: (integer) Transaction index within the block to start from.
* address: (data) Contract address whose storage is to be retrieved.
* startKey: (hash) The starting key for the storage range.
* limit: (integer) Number of storage entries to return.

### Request Example

URL (API Endpoints)\
https://go.getblock.io/\<ACCESS-TOKEN>/

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \  
--header 'Content-Type: application/json' \  
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_storageRangeAt",
    "params": [
        "0x340e1e44e8f6e6b0cd8d255d4fdaec2987bb073f02c62e068ed75d80f9890d5f",
        3,
        "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
        1
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
"method": "debug_storageRangeAt",
"params": ["0x340e1e44e8f6e6b0cd8d255d4fdaec2987bb073f02c62e068ed75d80f9890d5f", 3, "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", "0xc94770007dda54cF92009BFF0dE90c06F603a09f", 1],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object. Below is an example response for the debug\_storageRangeAt method:

```
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

* result: Contains the contract storage data for the specified range, or null if unavailable.
* status\_code: The HTTP status code for the request.
* message: An explanatory message, e.g., "Method not allowed."
* storage: An object containing key-value pairs of the storage.
*
  * hash: The hash value of the storage entry.
  * key: The storage key associated with the hash.
  * value: The storage value associated with the hash.
  * nextKey: The hash of the next key if additional storage entries are present within the range. Otherwise, not included.

### Use Case

The debug\_storageRangeAt method is invaluable for debugging smart contracts by allowing developers to inspect storage within a specified range. This is particularly useful when analyzing contract behavior or diagnosing issues related to on-chain storage. If a debug\_storageRangeAt error occurs, ensure that the parameters, such as blockHash and address, are correct. The provided debug\_storageRangeAt example demonstrates how to construct a proper request.

### Example Code

You can make requests to the debug\_storageRangeAt method programmatically using Python. Below is an example using the requests library:

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
    "method": "debug_storageRangeAt",
    "params": [
        "0x340e1e44e8f6e6b0cd8d255d4fdaec2987bb073f02c62e068ed75d80f9890d5f",
        3,
        "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
        1
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
    jsonrpc: "2.0",
    method: "debug_storageRangeAt",
    params: [
        "0x340e1e44e8f6e6b0cd8d255d4fdaec2987bb073f02c62e068ed75d80f9890d5f",
        3,
        "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95",
        "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
        1
    ],
    id: "getblock.io"
};

axios.post(url, data, { headers })
    .then(response => {
        console.log(response.data);
    })
    .catch(error => {
        console.error('Error:', error);
    });

```
{% endtab %}
{% endtabs %}

This script sends a request to the debug\_storageRangeAt method and prints the result. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 debug\_storageRangeAt method can also be used through Web3 libraries for Ethereum.

The Ethereum debug\_storageRangeAt method provides structured insights into contract storage. As part of the Core API Endpoints, it ensures developers have access to essential debugging tools. Each storage entry includes its key, value, and optional nextKey for further exploration. By supporting detailed storage data retrieval, this method integrates seamlessly with Ethereum's JSON RPC API for advanced debugging capabilities.

\\
