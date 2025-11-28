---
description: >-
  The web3_clientVersion method is part of the Ethereum JSON RPC Core API and
  retrieves the current version of the Ethereum client. It helps developers
  ensure compatibility.
---

# web3\_clientVersion - Ethereum

{% hint style="success" %}
The web3\_clientVersion method retrieves the current version of the Ethereum client, providing developers with essential information for compatibility and debugging.
{% endhint %}

The web3\_clientVersion method is part of the Ethereum JSON RPC Core API and is used to retrieve the current version of the Ethereum client. This method holds significant **value** for developers as it allows them to verify client and version information, ensuring compatibility with required features and functionalities. As part of the Core API Endpoints, it provides transparency about the client environment and includes detailed client version data. This simplifies monitoring, debugging, and integration with Ethereum nodes, offering quick and accurate client version verification and streamlining the development process.

### Supported Networks

The web3\_clientVersion RPC Ethereum method is supported across all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Request

URL

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
    "method": "web3_clientVersion",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "web3_clientVersion",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the version of the Ethereum client. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "Geth/v1.11.6-stable-ea9e62ca/linux-amd64/go1.20.3"
}
```

### Response Description

* result: A string indicating the version of the Ethereum client, including details such as the client name, version, operating system, and compiler version.

### Use Case

The web3\_clientVersion method is particularly useful for:

* Debugging: Identifying the client version to diagnose compatibility issues.
* Monitoring: Verifying that the client is running the expected version.
* Integration: Ensuring that the node supports the required features for application development.

In case of a web3\_clientVersion error, developers should check the node configuration and ensure that the node supports the JSON RPC API. A web3\_clientVersion example is included to demonstrate proper usage.

## Code Example

You can also make requests to the web3\_clientVersion method programmatically using Python. Below is an example using the requests library:

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
    "method": "web3_clientVersion",
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
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const data = {
  jsonrpc: '2.0',
  method: 'web3_clientVersion',
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

This Python script sends a request to the web3\_clientVersion method and prints the returned client version. Replace with your actual API token. The Web3 web3\_clientVersion method is also accessible through Web3 libraries for Ethereum, enabling developers to query client information efficiently.
