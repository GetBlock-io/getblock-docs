---
description: >-
  The net_listening method is part of the Ethereum JSON-RPC Core API and checks
  whether the Ethereum client is actively listening for network connections. It
  returns a boolean value.
---

# net\_listening - Ethereum

{% hint style="success" %}
The net\_listening method checks if the Ethereum client is actively listening for network connections.
{% endhint %}

The net\_listening method is part of the Ethereum JSON RPC Core API, used to determine whether the Ethereum client is actively listening for network connections. This method provides a simple boolean response, which is particularly useful for diagnosing network connectivity issues or verifying that the client is configured to accept connections

### Supported Networks

The net\_listening RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters. The request can be sent with an empty parameters array.
{% endhint %}

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
    "method": "net_listening",
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
"method": "net_listening",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing a boolean value indicating whether the client is listening for network connections. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": true
}
```

**Response Description**

* result: A boolean value that indicates whether the client is actively listening for network connections. true means the client is listening, and false means it is not.

### Use Case

The net\_listening method is particularly useful for developers and system administrators who need to verify the network status of their Ethereum node. For example, this method can help diagnose network connectivity issues or confirm that the client is ready to accept incoming connections. In case of a net\_listening error, verify that the Ethereum client is running and properly configured to listen for connections. An example of correct usage is included in this documentation under the net\_listening example.

### Code Example

You can also make requests to the net\_listening method programmatically using Python. Below is an example using the requests library:

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
    "method": "net_listening",
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
  method: 'net_listening',
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

This Python script sends a request to the net\_listening method and prints the returned boolean status. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 net\_listening method is also available in Web3 libraries for Ethereum, providing developers with a convenient interface to check the network status.

The Ethereum net\_listening method is an essential tool for diagnosing network issues and ensuring that the Ethereum client is actively accepting connections. This method is a key part of the Ethereum JSON RPC API and Core API Endpoints.
