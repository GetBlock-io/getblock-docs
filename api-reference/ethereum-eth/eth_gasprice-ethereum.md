---
description: >-
  Retrieve the current gas price using eth_gasPrice. Provides the median gas
  price based on the last 100 blocks, optimizing transaction fees and user
  experience on the Ethereum network
---

# eth\_gasPrice- Ethereum

{% hint style="success" %}
The eth\_gasPrice method is part of the Ethereum JSON-RPC API, which is the standard Core API for interacting with Ethereum nodes.
{% endhint %}

This method returns a percentile gas unit price for the most recent blocks in the Ethereum blockchain, which can be used to estimate transaction fees. By default, the gas price is calculated from the last 100 blocks, returning the 50th percentile (median) value. If no blocks are available, the value for --min-gas-price is returned, with the gas price being restricted between --min-gas-price and --api-gas-price-max. The method is designed to provide dynamic, up-to-date gas prices to optimize Ethereum transactions and improve overall user experience.

### Supported Networks

The eth\_gasPrice RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
The method does not require any parameters, as it automatically calculates the gas price based on the most recent blocks on the Ethereum network.
{% endhint %}

### Request Example

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request, you need to send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="JSON" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_gasPrice",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="WS" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_gasPrice",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object containing the current gas price in Wei. Below is an example of a typical response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x658ebc492"
}
```

### Response Description

result: The gas price in Wei, represented as a hexadecimal string. This value corresponds to the current estimated gas price for the Ethereum network based on the last blocks.

### Use Case

For DApp developers and users transacting on the Ethereum network, the eth\_gasPrice method provides a reliable estimate of the current transaction fees. By retrieving this gas price, developers can set the right gas limits and transaction fees for their smart contracts, ensuring faster and more cost-effective transactions.

### Code Example

You can also make requests to the eth\_gasPrice method programmatically using Python. Below is an example using the requests library

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
    "method": "eth_gasPrice",
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

// Define the API URL and access token
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_gasPrice',
  params: [],
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

This Python script sends a request to the eth\_gasPrice method and prints the returned gas price information. Make sure to replace with your actual API token.
