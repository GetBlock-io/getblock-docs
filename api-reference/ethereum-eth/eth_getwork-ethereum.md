---
description: >-
  The eth_getWork method returns the current block hash, seed hash, and target
  boundary condition, essential for mining software to find valid blocks. It's
  part of Ethereum's JSON-RPC Core API
---

# eth\_getWork - Ethereum

{% hint style="success" %}
This method returns the hash of the current block, the seed hash, and the required target boundary condition
{% endhint %}

The eth\_getWork method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. This method returns the hash of the current block, the seed hash, and the required target boundary condition. The eth\_getWork RPC Ethereum method is particularly useful for mining software that needs to obtain the necessary data for hashing in order to find valid blocks. If there is no mining work available, an eth\_getWork error will be returned.

### Supported Networks

The eth\_getWork RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters.
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
    "method": "eth_getWork",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{
    "jsonrpc": "2.0",
    "method": "eth_getWork",
    "params": [],
    "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with either the mining work data or an error message if no mining work is available. Below is an example of a typical error response:

```json
{
    "error": {
        "code": -32000,
        "message": "no mining work available yet"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```

### Response Description

* error: If there is no mining work available, an error object will be returned with details, including the error code and message.
* value: Represents the data or response values returned by the method.

### Use Case

The eth\_getWork method is primarily used by mining software to receive the current block's information and the hashing parameters needed to attempt finding a valid hash. This method provides miners with the block hash, the seed hash, and the required boundary target, which they use to find a valid nonce. In case of an eth\_getWork error, developers should verify the mining state and availability of work on the node. An eth\_getWork example is provided in the request section for reference.

### Code Example

You can also make requests to the eth\_getWork method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_getWork",
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

// Замените <ACCESS-TOKEN> на ваш реальный токен доступа
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = {
  'Content-Type': 'application/json'
};

const data = {
  jsonrpc: '2.0',
  method: 'eth_getWork',
  params: [],
  id: 'getblock.io'
};

// Отправка POST-запроса с использованием axios
axios.post(url, data, { headers })
  .then(response => {
    console.log('Ответ:', response.data);
  })
  .catch(error => {
    console.error('Ошибка:', error.message);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_getWork method and prints the returned work information or error. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_getWork method can also be used in Web3 libraries for Ethereum, providing an interface to interact with mining work for the network.

The Ethereum eth\_getWork method provides miners with the necessary information to start mining, making it an essential part of the Ethereum JSON RPC API and Core API Endpoints. It is useful for retrieving information about the block value, transaction, and parameters involved in mining operations.
