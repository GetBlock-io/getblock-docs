---
description: >-
  The eth_getUncleCountByBlockNumber method returns the number of uncles in a
  block by its block number, providing developers with insights into network
  consensus and block performance via Ethereum's
---

# eth\_getUncleCountByBlockNumber - Ethereum

{% hint style="success" %}
This method returns the number of uncles in a block that matches the specified block number.
{% endhint %}

The eth\_getUncleCountByBlockNumber method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. This method returns the number of uncles in a block that matches the specified block number. The eth\_getUncleCountByBlockNumber RPC Ethereum method provides an efficient way to retrieve uncle counts, which is useful for analyzing network consensus and block performance. The value returned can be used to better understand network conditions.

### Supported Networks

The eth\_getUncleCountByBlockNumber RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* QUANTITY | TAG: An integer representing the block index within the blockchain, or one of the string tags: latest, earliest, or pending, as described in the Block Parameter.
* parameters: Additional information or data that might be needed to refine the query.

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
    "method": "eth_getUncleCountByBlockNumber",
    "params": [
        "0xe8"
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
    "method": "eth_getUncleCountByBlockNumber",
    "params": [
        "0xe8"
    ],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the number of uncle blocks for the specified block number. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```

### Response Description

* result: The result contains the number of uncle blocks that were included in the given block.

### Use Case

The eth\_getUncleCountByBlockNumber method is particularly useful for developers and researchers interested in the consensus mechanism of Ethereum. By using the eth\_getUncleCountByBlockNumber method, they can determine how often uncles are included in blocks and analyze the impact on the blockchain. In case of an eth\_getUncleCountByBlockNumber error, developers should verify that the provided block number is valid. An eth\_getUncleCountByBlockNumber example can be found in this documentation to illustrate correct usage.

### Code Example

You can also make requests to the eth\_getUncleCountByBlockNumber method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getUncleCountByBlockNumber",
    "params": [
        "0xe8"
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
  method: 'eth_getUncleCountByBlockNumber',
  params: [
    '0xe8'
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

This Python script sends a request to the eth\_getUncleCountByBlockNumber method and prints the returned uncle block count. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_getUncleCountByBlockNumber method can also be used in Web3 libraries for Ethereum, providing an interface to access uncle block data for various use cases, including blockchain research and network performance analysis.

The Ethereum eth\_getUncleCountByBlockNumber method provides a valuable tool for querying uncle counts, making it an essential part of the Ethereum JSON RPC API and Core API Endpoints.

The value of uncle counts can provide insights into network stability and miner behavior, which are crucial for maintaining a healthy blockchain ecosystem.
