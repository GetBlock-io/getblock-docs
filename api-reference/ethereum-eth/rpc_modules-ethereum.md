---
description: >-
  The rpc_modules method is part of the Ethereum JSON-RPC Core API and lists all
  enabled APIs and their versions on the Ethereum node. It helps developers
  verify module availability .
---

# rpc\_modules - Ethereum

{% hint style="success" %}
The rpc\_modules method lists all enabled JSON-RPC APIs and their versions on the Ethereum node, helping developers verify module availability and compatibility.
{% endhint %}

The rpc\_modules method is part of the Ethereum JSON RPC Core API, used to list all enabled APIs and their corresponding versions on the Ethereum node. This method is particularly relevant when developers are working with specific modules, such as block or transaction, to ensure compatibility with Ethereum node configurations. Understanding the versioning and availability of these modules simplifies integration and reduces errors during implementation. The response includes all relevant module details, which can be used to validate the value of specific module interactions.

### Supported Networks

The rpc\_modules RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Request Example

URL (API Endpoint)

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
    "method": "rpc_modules",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "rpc_modules",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object containing a list of enabled APIs and their respective versions. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "debug": "1.0",
        "eth": "1.0",
        "net": "1.0",
        "rpc": "1.0",
        "txpool": "1.0",
        "web3": "1.0"
    }
}
```

### Response Description

* result: An object containing the names of the enabled APIs as keys and their corresponding versions as values.

### Use Case

The rpc\_modules method is essential for developers who need to verify which APIs are enabled on an Ethereum node. This can be useful for debugging, setting up environments, or ensuring compatibility with specific features. In case of an rpc\_modules error, developers should confirm that the node supports this method and is configured correctly. An rpc\_modules example is included in this documentation to demonstrate the correct usage of this method.

## Example Code

You can also make requests to the rpc\_modules method programmatically using Python. Below is an example using the requests library:

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
    "method": "rpc_modules",
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

const data = {
  jsonrpc: '2.0',
  method: 'rpc_modules',
  params: [],
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

This Python script sends a request to the rpc\_modules method and prints the returned list of enabled APIs and their versions. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 rpc\_modules method can also be accessed through Web3 libraries for Ethereum, offering a convenient way to retrieve module information programmatically.

The Ethereum rpc\_modules method is a key tool for developers working with the Ethereum JSON RPC API, enabling them to identify available APIs and their versions quickly. As part of the Core API Endpoints, this method enhances compatibility and debugging workflows by providing transparent module information.

\\
