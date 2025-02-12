---
description: >-
  The eth_unsubscribe method is part of the Ethereum JSON-RPC Core API and is
  used to cancel a specified subscription by its ID. It helps developers manage
  active subscriptions, optimizing resource.
---

# eth\_unsubscribe - Ethereum

{% hint style="success" %}
This method cancels a specified subscription by its ID
{% endhint %}

The eth\_unsubscribe method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_unsubscribe RPC Ethereum method is particularly useful for managing active subscriptions and ensuring resource efficiency by removing unnecessary subscriptions.The method provides value to developers by optimizing resource usage.

### Supported Networks

The eth\_unsubscribe RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: A hex string representing the subscription ID that was previously generated with the eth\_subscribe method.
* parameters: Additional input data used to specify the subscription ID for cancellation.

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
    "method": "eth_unsubscribe",
    "params": [
        "0xe5af64ddfd365b4632988c5935cfedb7"
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
"method": "eth_unsubscribe",
"params": ["0xe5af64ddfd365b4632988c5935cfedb7"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object indicating the success or failure of the unsubscription. Below is an example response:

```json
{
    "error": {
        "code": -32000,
        "message": "subscription not found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```

**Response Description**

* result: Returns true if the subscription was successfully canceled or false otherwise.
* error: Provides an error message, such as "subscription not found," if the subscription ID is invalid or does not exist.Each response helps track block states or errors that might occur during the cancellation process.

### Use Case

The eth\_unsubscribe method is essential for developers who need to manage active subscriptions created using the eth\_subscribe method. By unsubscribing from unneeded events, developers can optimize resource usage and maintain efficient connections with the Ethereum node. In case of an eth\_unsubscribe error, ensure that the subscription ID provided in the parameters matches an existing subscription.

An eth\_unsubscribe example demonstrates how to properly use this method to cancel subscriptions effectively.This approach ensures fewer unnecessary transactions being sent to the node, improving overall efficiency.

### Code Example

You can also make requests to the eth\_unsubscribe method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_unsubscribe",
    "params": [
        "0xe5af64ddfd365b4632988c5935cfedb7"
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

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/'; 

const data = {
  jsonrpc: '2.0',
  method: 'eth_unsubscribe',
  params: ['0xe5af64ddfd365b4632988c5935cfedb7'],
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

This Python script sends a request to the eth\_unsubscribe method and prints the returned status of the unsubscription. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_unsubscribe method can also be used in Web3 libraries for Ethereum, providing an interface to manage subscriptions in decentralized applications.

The Ethereum eth\_unsubscribe method provides a reliable way to cancel subscriptions, making it a critical part of the Ethereum JSON RPC API and Core API Endpoints.
