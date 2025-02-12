---
description: >-
  Retrieve the compiled bytecode of a smart contract at a specific address using
  eth_getCode. Essential for verifying contract deployment and analyzing
  bytecode in Web3 development.
---

# eth\_getCode - Ethereum

{% hint style="success" %}
The eth\_getCode RPC Ethereum method is part of the Ethereum JSON RPC protocol. It retrieves the compiled code of a smart contract located at a specific address.
{% endhint %}

The response is returned as a hexadecimal value, representing the contract's bytecode.

This method is commonly used by developers working with Web3 and smart contract platforms to verify if a contract is deployed at a specific address or to fetch the bytecode for analysis. Ethereum eth\_getCode is an essential part of the Ethereum endpoints for blockchain interaction via JSON-RPC.

### Supported Networks

The eth\_getCode RPC Ethereum method supports the following network types

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* **DATA**: A 20-byte address of the contract whose code you want to retrieve.
* **QUANTITY or TAG**:
  * `latest`: Retrieves data from the most recently mined block.
  * `earliest`: Retrieves data from the genesis block.
  * `pending`: Retrieves data from the block currently being mined.

### Request Example

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here is an eth\_getCode example of how to use the method in a JSON-RPC request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0xa50a51c09a5c451c52bb714527e1974b686d8e77",
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
"method": "eth_getCode",
"params": ["0xa50a51c09a5c451c52bb714527e1974b686d8e77", "latest"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

* method: Specifies the API method (eth\_getCode).
* params: Contains:
  1. The 20-byte contract address.
  2. The block parameter indicating the block to reference (latest).

id: A unique identifier for the request.

### Response Example

The response from the eth\_getBlockTransactionCountByNumber method will look like this:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```

### Response Description

* id: Matches the id from the request to help correlate responses.
* jsonrpc: Confirms the JSON-RPC protocol version ("2.0").
* result:
*
  * A hexadecimal string containing the smart contract’s compiled bytecode if the contract exists.
  * "0x" if no smart contract is present at the queried address.

#### Error Response Example

```
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "error": {
        "code": -32602,
        "message": "Invalid params"
    }
}
```

\
Developers may encounter common issues such as an eth\_getCode error, which typically occurs due to invalid parameters or an inaccessible block. Always ensure your request includes a valid contract address and block parameter.

### Use Case

Checking for the presence of a smart contract\
Use eth\_getCode to verify whether a smart contract is deployed at a given address. If the result is "0x", no contract exists at the address.

Fetching contract bytecode\
Retrieve the bytecode of a deployed smart contract for further analysis, such as reverse engineering or debugging.

Integrating with Web3\
This method is commonly used by developers working with Web3 eth\_getCode to interact with Ethereum smart contracts programmatically.

### Code Example

Here is an example request for getting contract code using different programming languages

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
    "method": "eth_getCode",
    "params": [
        "0xa50a51c09a5c451c52bb714527e1974b686d8e77",
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
  method: 'eth_getCode',
  params: [
    '0xa50a51c09a5c451c52bb714527e1974b686d8e77',
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

Request Setup:The API method eth\_getCode is invoked with the necessary parameters (address and block). Error Handling:Handles HTTP errors (e.g., server not reachable) and JSON-RPC-specific errors (e.g., invalid parameters or method issues). Result Usage:If a contract is present, logs its bytecode value for further processing.
