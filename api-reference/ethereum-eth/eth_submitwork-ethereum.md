---
description: >-
  The eth_submitWork method is part of the Ethereum JSON-RPC API and is used to
  submit a Proof of Work (Ethash) solution.
---

# eth\_submitWork-Ethereum

{% hint style="success" %}
This method is commonly utilized by mining software, such as Ethminer, to validate block mining efforts and contribute to the Ethereum blockchain.
{% endhint %}

The eth\_submitWork method is part of the Ethereum JSON-RPC API and is used to submit a Proof of Work (Ethash) solution.

### Supported Networks

The eth\_submitWork RPC Ethereum method is supported on the following network types

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

The method accepts the following parameters:

1. DATA (8 Bytes): The retrieved nonce in hexadecimal format.
2. DATA (32 Bytes): The hash of the block header (PoW-hash) in hexadecimal format.
3. DATA (32 Bytes): The mix digest in hexadecimal format.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_submitWork endpoint using JSON-RPC, use the following examples:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_submitWork",
    "params": [
        "0x0000000000000001",
        "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
        "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef"
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
"method": "eth_submitWork",
"params": ["0x0000000000000001", "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef", "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response indicates whether the Proof of Work submission was successful.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```

### Response Description

* result: A boolean value indicating the success (true) or failure (false) of the Proof of Work submission.
* value: Not directly included in the response, but represents the outcome of the Proof of Work validation attempt.

### Use Case

The eth\_submitWork RPC Ethereum method is widely used in mining software and tools to:

* Submit valid Proof of Work solutions.
* Participate in the Ethereum blockchain mining process.
* Validate computational contributions to the network.

For instance, a Web3 mining application may use the Ethereum eth\_submitWork method to send PoW solutions and earn mining rewards based on successful validations.

### Code Example

Here is an eth\_submitWork example of how to query the method using Python and JavaScript:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API URL and headers
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}

# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "eth_submitWork",
    "params": [
        "0x0000000000000001",
        "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
        "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef"
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

// Define the API URL and headers
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_submitWork',
  params: [
    '0x0000000000000001',
    '0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef',
    '0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef'
  ],
  id: 'getblock.io'
};

// Send the POST request
axios.post(url, data, { headers })
  .then(response => {
    // Print the result
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });

```
{% endtab %}
{% endtabs %}

#### Common Errors

When using the eth\_submitWork RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* eth\_submitWork error: This could happen if the Proof of Work solution is invalid or improperly formatted.
* Incorrect Nonce, Hash, or Mix Digest: Verify that all parameters are valid hexadecimal strings with the correct length.

By integrating the Web3 eth\_submitWork method into your mining software, you can ensure efficient submission and validation of Proof of Work solutions. Use this core API method to contribute to the Ethereum blockchain and participate in its decentralized consensus process.
