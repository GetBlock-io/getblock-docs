---
description: >-
  The web3_sha3 method is part of the Ethereum JSON RPC Core API and computes
  the Keccak-256 hash of the provided input data, returning a
  hexadecimal-encoded hash string.
---

# web3\_sha3 - Ethereum

{% hint style="success" %}
The web3\_sha3 method computes the Keccak-256 hash of the given input data, returning a hexadecimal-encoded hash string.
{% endhint %}

The web3\_sha3 method is part of the Ethereum JSON RPC Core API, designed to return a SHA3 hash of the specified data. Note that the result value is a Keccak-256 hash, not the standardized SHA3-256, which is important for Ethereum-specific cryptographic operations.

### Supported Networks

The web3\_sha3 RPC Ethereum method is universally supported across all Ethereum networks, including:

* Mainnet
* Testnets: Sepolia, Hoodi

### Parameters

* DATA: Data to convert to a SHA3 hash.
* parameters: None.

### Request

URL&#x20;

```
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
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c00"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "web3_sha3",
"params": ["0x68656c6c6f20776f726c00"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the Keccak-256 hash of the input data. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5e39a0a66544c0668bde22d61c47a8710000ece931f13b84d3b2feb44ec96d3f"
}
```

### Response Description

* result: The Keccak-256 hash of the provided data, represented as a hexadecimal string.

### Use Case

The web3\_sha3 method is crucial for developers working with Ethereum-based cryptographic operations, such as validating signatures or creating unique identifiers. It can be used to hash arbitrary data to ensure data integrity or to generate unique keys. In case of a web3\_sha3 error, developers should verify that the input data is properly formatted as a hexadecimal string.

An illustrative web3\_sha3 example is provided in this documentation to demonstrate the method's correct usage.

## Code Example

You can also make requests to the web3\_sha3 method programmatically using Python. Below is an example using the requests library:

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
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c00"
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
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const data = {
  jsonrpc: '2.0',
  method: 'web3_sha3',
  params: [
    "0x68656c6c6f20776f726c00"
  ],
  id: 'getblock.io',
};

axios.post(url, data, {
  headers: {
    'Content-Type': 'application/json',
  },
})
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the web3\_sha3 method and prints the returned hash value. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 web3\_sha3 method can also be utilized through Web3 libraries for Ethereum to integrate hashing capabilities directly into decentralized applications.

The Ethereum web3\_sha3 method is an integral part of the Ethereum JSON RPC API, allowing developers to leverage Keccak-256 hashing in their projects. As a Core API Endpoint, it ensures secure and reliable hashing operations while maintaining compatibility across different Ethereum environments.
