---
description: >-
  The eth_hashrate method returns the mining hashrate in hashes per second,
  reflecting the mining performance of the node. It is part of Ethereum's
  JSON-RPC Core API and helps monitor network security
---

# eth\_hashrate - Ethereum

{% hint style="success" %}
This method returns the number of hashes per second with which the node is mining
{% endhint %}

The eth\_hashrate method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. This method returns the number of hashes per second with which the node is mining. When the stratum server is enabled, the eth\_hashrate RPC Ethereum method returns the cumulative hashrate of all sealers reporting their hashrate. This information is crucial for monitoring the mining performance and overall network security.

### Supported Networks

The eth\_hashrate RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
No parameters are required for this method
{% endhint %}

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
    "method": "eth_hashrate",
    "params": [],
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
    "method": "eth_hashrate",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the mining hashrate. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```

### Response Description

* result: The result contains the current mining hashrate, represented as a hexadecimal string value

### Use Case

The eth\_hashrate method is particularly useful for developers and miners who need to monitor the hashrate of their node. By using the eth\_hashrate method, developers can determine how effectively their mining hardware is performing. In case of an eth\_hashrate error, developers should check if the mining process is properly set up and that the node is connected to the network. An eth\_hashrate example is provided in this documentation to illustrate the correct usage.

### Code Example

You can also make requests to the eth\_hashrate method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_hashrate",
    "params": [],
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
  method: 'eth_hashrate',
  params: [],
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

This Python script sends a request to the eth\_hashrate method and prints the returned hashrate value. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_hashrate method can also be used in Web3 libraries for Ethereum, providing an interface to access the mining hashrate data for various use cases, including monitoring mining performance and optimizing mining operations.

The Ethereum eth\_hashrate method is an important part of the Ethereum JSON RPC API and Core API Endpoints, providing real-time information about the mining capabilities of a node. This method can be useful in scenarios where monitoring mining efficiency or determining overall network participation is needed. Additionally, developers can use this method to collect metrics on block production and transaction processing efficiency, making it a valuable tool in the Ethereum development ecosystem.
