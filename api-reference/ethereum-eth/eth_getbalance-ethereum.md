---
description: >-
  Check the balance of a specified Ethereum address with the balance method.
  Query account balance at specific blocks or transaction states, supporting
  various time points like latest or pending
---

# eth\_getBalance - Ethereum

{% hint style="success" %}
The eth\_getBalance method is part of the Ethereum JSON-RPC API and is used to retrieve the balance of a specified Ethereum address.
{% endhint %}

This method allows you to check the account balance at a specific block or transaction state, making it useful for various DApps and Ethereum-based applications. The method can be queried with the latest, earliest, or pending block tags to get the balance at different points in time. It provides a convenient API method to access the account balance, supporting both request and parameters such as the account address and block tag.

### Supported Networks

The eth\_getBalance RPC Ethereum method works on the following Ethereum network types

* **Mainnet**
* **Testnet**: Sepolia, Holesky

### Parameters

DATA: A 20-byte Ethereum account address to retrieve the balance for. The address must be in hexadecimal format, prefixed with 0x.\
QUANTITY|TAG: A string representing the block number or one of the following tags:

* latest (default): Retrieves the balance of the account at the latest block.
* earliest: Retrieves the balance at the first block (genesis block).
* pending: Retrieves the balance of the account in the pending block.

### Request

URL

{% code fullWidth="false" %}
```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}

To make a request to the eth\_getBalance method, you can use the following curl command. The request sends a JSON-RPC query to the Ethereum network via the GetBlock API

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
        "latest"
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
"method": "eth_getBalance",
"params": ["0xfe3b557e8fb62b89f4916b721be55ceb828dbd73", "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response will contain the balance of the specified Ethereum address, represented as a hexadecimal string in Wei. Below is an example response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x36ea"
}
```

### Response Description

* result: The balance of the Ethereum address in Wei, given as a hexadecimal string. In the example, the balance is 0x36ea, which is the equivalent of 14006 Wei.

### Use Case

The eth\_getBalance method is often used in decentralized applications (DApps), including those integrated with MetaMask, to check the balance of Ethereum accounts before performing transactions. For example, DApps may need to verify that a user has sufficient funds to complete a transaction or make a purchase. By using the eth\_getBalance method, developers can easily retrieve the balance of an account at any given block or the latest state of the Ethereum blockchain. When interacting with MetaMask, this method can be called to display the user's wallet balance in real-time, providing a seamless experience for users during transactions.

### Code Example

To retrieve the balance of an address using axios, you can use the eth\_getBalance method as follows

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
    "method": "eth_getBalance",
    "params": [
        "0xfe3b557e8fb62b89f4916b721be55ceb828dbd73",
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

// Define the API URL and access token
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_getBalance',
  params: [
    '0xfe3b557e8fb62b89f4916b721be55ceb828dbd73',
    'latest'
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

This code will retrieve the balance of the specified address on the Ethereum mainnet and print it in ETH.

### Common Errors

eth\_getBalance error: Common issues when using the eth\_getBalance method include incorrect address format, network connectivity problems, or invalid block/tag parameters. Ensure that the address is valid, and the network you are querying is correct.
