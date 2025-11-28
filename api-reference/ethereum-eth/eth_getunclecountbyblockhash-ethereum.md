---
description: >-
  The eth_getUncleCountByBlockHash method returns the number of uncle blocks in
  a block by its hash, aiding developers in analyzing network consensus and
  performance through Ethereum's JSON-RPC Core API
---

# eth\_getUncleCountByBlockHash - Ethereum

{% hint style="success" %}
This method returns the number of uncle blocks in a block that matches the given block hash
{% endhint %}

The eth\_getUncleCountByBlockHash method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. This method returns the number of uncle blocks in a block that matches the given block hash. The eth\_getUncleCountByBlockHash RPC Ethereum method is used to retrieve information about uncle blocks, which helps in understanding network consensus and performance.

### Supported Networks

The eth\_getUncleCountByBlockHash RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

* DATA: The 32-byte block hash.
* parameters: Additional information or data that might be needed to refine the query.

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
    "method": "eth_getUncleCountByBlockHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{
    "jsonrpc": "2.0",
    "method": "eth_getUncleCountByBlockHash",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
    ],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the number of uncle blocks for the specified block hash. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x1"
}
```

### Response Description

* result: The result contains the number of uncle blocks that were included in the given block.

### Use Case

The eth\_getUncleCountByBlockHash method is particularly useful for developers and researchers interested in the consensus mechanism of Ethereum. By using the eth\_getUncleCountByBlockHash method, they can determine how often uncles are included and understand the impact on the blockchain. In case of an eth\_getUncleCountByBlockHash error, developers should check the provided block hash for accuracy. An eth\_getUncleCountByBlockHash example can be found in this documentation to illustrate correct usage.

### Code Example

You can also make requests to the eth\_getUncleCountByBlockHash method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getUncleCountByBlockHash",
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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = {
  'Content-Type': 'application/json'
};

const data = {
  jsonrpc: '2.0',
  method: 'eth_getUncleCountByBlockHash',
  params: [
    '0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7'
  ],
  id: 'getblock.io'
};

axios.post(url, data, { headers })
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getUncleCountByBlockHash method and prints the returned uncle block value. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_getUncleCountByBlockHash method can also be used in Web3 libraries for Ethereum, providing an interface to access uncle block data for various use cases, including blockchain research and network performance analysis.

The Ethereum eth\_getUncleCountByBlockHash method provides a valuable tool for querying uncle counts, making it an essential part of the Ethereum JSON RPC API and Core API Endpoints.
