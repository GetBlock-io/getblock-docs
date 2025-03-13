---
description: >-
  The debug_batchSendRawTransaction method is part of the Ethereum JSON RPC Core
  API and enables the submission of multiple raw transactions in a single batch.
---

# debug\_batchSendRawTransaction - Ethereum

{% hint style="success" %}
The debug\_batchSendRawTransaction method allows sending multiple raw transactions in a single batch, enabling efficient submission of transactions to the Ethereum network.
{% endhint %}

The debug\_batchSendRawTransaction method is part of the Ethereum JSON RPC Core API, designed to send a list of signed transactions in a single request. This method allows developers to efficiently load a network with multiple transactions simultaneously, offering the same functionality as calling eth\_sendRawTransaction multiple times. However, this method is not allowed by default and may return an error if invoked.

### Supported Networks

The debug\_batchSendRawTransaction RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Request

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
    "method": "debug_batchSendRawTransaction",
    "params": [
        [
            "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
        ]
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
"method": "debug_batchSendRawTransaction",
"params": [["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"]],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with an error message, as this method is not allowed by default. Below is an example of a typical response:

```json
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

* result: Null, indicating the method is not allowed.
* status\_code: HTTP status code 405, which means the method is not allowed.
* message: A description of the error, "Method not allowed."

### Use Case

The debug\_batchSendRawTransaction method is useful for developers aiming to test or stress-load the Ethereum network by sending multiple transactions at once. However, due to its restricted nature, it is commonly disabled in production environments. In case of a debug\_batchSendRawTransaction error, developers should confirm the method is enabled and ensure proper signed transaction data is provided. A debug\_batchSendRawTransaction example is included in this documentation for reference.

## Code Example

You can also make requests to the debug\_batchSendRawTransaction method programmatically using Python. Below is an example using the requests library:

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
    "method": "debug_batchSendRawTransaction",
    "params": [
        [
            "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
        ]
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
  method: 'debug_batchSendRawTransaction',
  params: [
    [
      "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
    ]
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

This Python script demonstrates how to call the debug\_batchSendRawTransaction method. Ensure that \<ACCESS-TOKEN> is replaced with a valid API token. The Web3 debug\_batchSendRawTransaction method can also be utilized via Web3 libraries for Ethereum, providing a streamlined way to send bulk transactions programmatically.

The Ethereum debug\_batchSendRawTransaction method is a specialized tool for sending multiple transactions in one request, offering efficiency and reduced overhead for bulk transaction scenarios. As part of the Ethereum JSON RPC API and Core API Endpoints, this method is ideal for testing and development environments, where bulk transaction operations are required. The method accepts transaction parameters and returns a value indicating the success or failure of the operation.
