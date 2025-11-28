---
description: >-
  The net_version method is part of the Ethereum JSON-RPC Core API and returns
  the network ID of the Ethereum client. It helps identify the current network
  the client is connected to, such as Mainnet.
---

# net\_version - Ethereum

{% hint style="success" %}
The net\_version method returns the network ID of the Ethereum client, identifying the current network (e.g., Mainnet, Sepolia, or a test network) it is connected to.
{% endhint %}

The net\_version method is part of the Ethereum JSON RPC Core API, used to retrieve the network ID of the Ethereum client. This network ID is crucial for identifying the current Ethereum network that the client is connected to, such as Mainnet, Sepolia, or other test networks.

### Supported Networks

The net\_version RPC Ethereum method supports the following network types:

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
{% tab title="JSON" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "net_version",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="WS" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "net_version",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the network ID. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "1"
}
```

**Response Description**

* result: A string representing the network ID. For example:
  * "1": Mainnet
  * "11155111": Sepolia
  * "3": Ropsten (deprecated)
  * "4": Rinkeby (deprecated)

### Use Case

The net\_version method is essential for developers who need to verify the network their Ethereum client is connected to. This is particularly useful in multi-network applications where the client must dynamically switch between networks. In case of a net\_version error, ensure the client is correctly configured and connected to the desired network. An example of proper usage is included under the net\_version example in this documentation.

### Code Example

You can also make requests to the net\_version method programmatically using Python. Below is an example using the requests library:

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
    "method": "net_version",
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
import axios from 'axios'; // Ensure axios is installed: npm install axios

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const data = {
  jsonrpc: '2.0',         
  method: 'net_version',  
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

This Python script sends a request to the net\_version method and prints the returned network ID. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 net\_version method can also be accessed via Web3 libraries for Ethereum, providing a simplified way to query network information.

The Ethereum net\_version method is a key tool for developers working with the Ethereum JSON RPC API. By identifying the current network through its ID, this method ensures that transactions and operations are executed on the intended blockchain. As part of the Core API Endpoints, it plays a critical role in maintaining network integrity and compatibility.As part of the Core API Endpoints, it plays a critical role in maintaining network integrity and compatibility, while ensuring accurate block and transaction management.
