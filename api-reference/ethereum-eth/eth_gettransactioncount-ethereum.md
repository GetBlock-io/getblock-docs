---
description: >-
  Retrieve the number of transactions sent from a specified address using
  eth_getTransactionCount. Essential for tracking account activity and
  determining the next nonce for transactions on the Ethereum
---

# eth\_getTransactionCount - Ethereum

{% hint style="success" %}
This method returns the number of transactions sent from a specified address.
{% endhint %}

The eth\_getTransactionCount method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes.By using the pending tag, developers can get the next account nonce that has not been used by any pending transactions. The eth\_getTransactionCount RPC Ethereum method is essential for tracking account activity and determining the next available nonce for new transactions.

### Supported Networks

The eth\_getTransactionCount RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The 20-byte account address.
* QUANTITY | TAG: An integer representing a block number or one of the string tags latest, earliest, or pending as described in the Block Parameter.

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
    "method": "eth_getTransactionCount",
    "params": [
        "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
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
"method": "eth_getTransactionCount",
"params": ["0xc94770007dda54cF92009BFF0dE90c06F603a09f", "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the number of transactions sent from the specified address. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x219"
}
```

### Response Description

* result: The number of transactions value sent from the specified address, represented as a hexadecimal string.

### Use Case

The eth\_getTransactionCount method is particularly useful for developers who need to determine the next available nonce for a given account. This is important when constructing new transactions, as the nonce must be unique for each transaction sent by an address. Using the eth\_gettransactioncount pending tag allows developers to retrieve the nonce that will be used for the next transaction that has not yet been mined. In case of an eth\_getTransactionCount error, developers should verify that the provided account address and block tag are correct. An eth\_getTransactionCount example can help illustrate how to properly use this method.

### Code Example

You can also make requests to the eth\_getTransactionCount method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getTransactionCount",
    "params": [
        "0xc94770007dda54cF92009BFF0dE90c06F603a09f",
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

// Replace <ACCESS-TOKEN> with your actual Access Token
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const payload = {
  jsonrpc: '2.0',
  method: 'eth_getTransactionCount',
  params: [
    '0xc94770007dda54cF92009BFF0dE90c06F603a09f', // Address
    'latest', // Block parameter
  ],
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, payload, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    console.log('Transaction Count Response:', response.data);
  })
  .catch((error) => {
    console.error('Error:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getTransactionCount method and prints the returned transaction count. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The eth\_gettransactioncount pending tag is useful for retrieving the next available nonce that has not been used by any pending transactions.

The Web3 eth\_getTransactionCount method can also be used in Web3 libraries for Ethereum, providing an interface to access transaction count data for various use cases, including constructing new transactions and debugging account activity.

The Ethereum eth\_getTransactionCount method provides a reliable way to query the number of transactions for a given account address across different network types, making it a versatile tool for developers working with Ethereum's JSON RPC API and Core API Endpoints.
