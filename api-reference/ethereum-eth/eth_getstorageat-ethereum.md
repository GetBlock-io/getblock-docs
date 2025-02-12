---
description: >-
  Retrieve the value of a storage position at a specified address using
  eth_getStorageAt. Essential for accessing specific data stored within smart
  contracts on the Ethereum blockchain
---

# eth\_getStorageAt - Ethereum

{% hint style="success" %}
This method returns the value of a storage position at a specified address, allowing developers to access specific data stored within smart contracts.
{% endhint %}

The Ethereum eth\_getStorageAt method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_getStorageAt RPC Ethereum method is essential for retrieving on-chain data directly from the storage of a contract.

### Supported Networks

The eth\_getStorageAt RPC Ethereum method supports the following network types

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 20-byte address of the storage (account or contract) for which to retrieve the storage value.
* DATA: The integer index of the storage position to access.
* QUANTITY | TAG: An integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl, eth\_getStorageAt example:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": [
        "0x9EC55d57208cb28a7714A2eA3468bD9d5bB15125",
        "0x0",
        "latest"
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
"method": "eth_getStorageAt",
"params": ["0x9EC55d57208cb28a7714A2eA3468bD9d5bB15125", "0x0", "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the value at the specified storage position. Below is an example of a typical response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

### Response Description

* result: The value at the specified storage position, represented as a hexadecimal string.

### Use Case

The eth\_getStorageAt method is particularly useful for developers who need to retrieve specific storage values from smart contracts on the Ethereum blockchain. This can be used for debugging or for accessing critical data stored within a contract. In case of an eth\_getStorageAt error, developers should ensure that the provided address, storage index, and block tag are correct.

### Code Example

You can also make requests to the eth\_getStorageAt method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getStorageAt",
    "params": [
        "0x9EC55d57208cb28a7714A2eA3468bD9d5bB15125",
        "0x0",
        "latest"
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

// Replace <ACCESS-TOKEN> with your actual API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getStorageAt',
  params: [
    '0x9EC55d57208cb28a7714A2eA3468bD9d5bB15125', // Address
    '0x0', // Storage slot
    'latest', // Block tag
  ],
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, requestBody, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Storage Value:', response.data.result);
  })
  .catch((error) => {
    console.error('Error fetching storage value:', error.message);
  });

```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getStorageAt method and prints the returned storage value. Make sure to replace \<ACCESS-TOKEN> with your actual API token.

Web3 eth\_getStorageAt libraries , providing an interface to access contract storage data for various use cases, including contract interaction, debugging, and data analysis.
