---
description: >-
  The net_peerCount method is part of the Ethereum JSON-RPC Core API and returns
  the number of peers connected to the Ethereum client. It provides a
  hexadecimal response.
---

# net\_peerCount - Ethereum

{% hint style="success" %}
The net\_peerCount method returns the number of peers currently connected to the Ethereum client.
{% endhint %}

The net\_peerCount method is part of the Ethereum JSON RPC Core API, used to retrieve the number of peers currently connected to the Ethereum client. This method provides a hexadecimal response representing the peer count, which is essential for monitoring the health and connectivity of the client.

### Supported Networks

The net\_peerCount RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Request

URL

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
    "method": "net_peerCount",
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
"method": "net_peerCount",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the current peer count. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x10"
}
```

**Response Description**

* result: A hexadecimal value representing the number of peers currently connected to the client.

### Use Case

The net\_peerCount method is particularly useful for developers and system administrators who need to monitor the network connectivity of their Ethereum node. A high peer count indicates good connectivity, while a low or zero count may suggest issues with the node’s network configuration. In case of a net\_peerCount error, ensure that the client is properly configured to connect to peers. An example of correct usage is provided in this documentation under the net\_peerCount example. A low peer count may also delay transaction propagation, affecting performance.

### Code Example

You can also make requests to the net\_peerCount method programmatically using Python. Below is an example using the requests library:

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
    "method": "net_peerCount",
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
  method: 'net_peerCount',
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

This Python script sends a request to the net\_peerCount method and prints the returned peer count. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 net\_peerCount method is also available in Web3 libraries for Ethereum, providing a convenient interface for developers to monitor peer connectivity.

The Ethereum net\_peerCount method is a key tool for assessing the network connectivity of an Ethereum client. By providing the number of connected peers, this method helps ensure that the client is fully operational and well-connected. This functionality is a vital part of the Ethereum JSON RPC API and Core API Endpoints.
