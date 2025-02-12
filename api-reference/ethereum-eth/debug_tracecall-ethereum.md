---
description: >-
  The debug_traceCall method is part of the Ethereum JSON RPC Core API and is
  used for advanced debugging. This method returns a transaction trace.
---

# debug\_traceCall - Ethereum

{% hint style="success" %}
The debug\_traceCall method is part of the Ethereum JSON RPC Core API and is designed for advanced debugging purposes
{% endhint %}

This method returns a transaction trace object, providing detailed insights into the transaction execution process. Developers can use this method to debug specific transaction calls in the context of a specified block.

### Supported Networks

The debug\_traceCall RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

The debug\_traceCall method accepts the following parameters:

* Object: (None) The transaction call object, specifying details such as the recipient address and value.
* DATA: (None) The block number in hex format, tags (latest, earliest, pending), or the block hash.
* DATA: (None) The type of tracer, such as callTracer or other supported tracer types.

### Request Example

URL (API Endpoint)\
https://go.getblock.io/\<ACCESS-TOKEN>/

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [
        {
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567"
        },
        "finalized",
        {
            "tracer": "callTracer"
        }
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
"method": "debug_traceCall",
"params": [{"to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567"}, "finalized", {"tracer": "callTracer"}],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object. Below is an example response for the debug\_traceCall method:

```json
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

* result: null if the method is not allowed or unavailable.
* status\_code: The HTTP status code for the request.
* message: A descriptive message explaining the response, e.g., "Method not allowed."

#### Returns

If successful, the method provides a detailed trace of the transaction call, including:

* block: The block details where the transaction is executed.
* transaction: The transaction trace object, containing opcode-level execution details.
* parameters: Detailed information about the transaction’s inputs and outputs.
* value: The value transferred or modified during the transaction execution.

### Use Case

The debug\_traceCall method is an essential tool for developers requiring granular debugging of Ethereum transactions. It is particularly useful for analyzing contract interactions, identifying gas inefficiencies, and diagnosing transaction failures. If a debug\_traceCall error occurs, ensure the provided transaction object, block number, or hash is correct. The debug\_traceCall example demonstrates how to construct a valid request.

### Example Code

Here is an example of how to call the debug\_traceCall method programmatically using Python:

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
    "method": "debug_traceCall",
    "params": [
        {
            "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567"
        },
        "finalized",
        {
            "tracer": "callTracer"
        }
    ],
    "id": "getblock.io"
}


# Send the POST request
response = requests.post(url, headers=headers, data=json.dumps(data))


# Parse the JSON response
response_data = response.json()


# Print the result
print(json.dumps(response_data, indent=4))Pyth
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/'; // Замените <ACCESS-TOKEN> на ваш токен доступа
const headers = {
    'Content-Type': 'application/json'
};
const data = {
    jsonrpc: "2.0",
    method: "debug_traceCall",
    params: [
        {
            to: "0xd46e8dd67c5d32be8058bb8eb970870f07244567"
        },
        "finalized",
        {
            tracer: "callTracer"
        }
    ],
    id: "getblock.io"
};

axios.post(url, data, { headers })
    .then(response => {
        console.log(response.data);
    })
    .catch(error => {
        console.error('Error:', error);
    });

```
{% endtab %}
{% endtabs %}

This Python script demonstrates how to interact with the debug\_traceCall method programmatically. Replace \<ACCESS-TOKEN> with your actual API key. The Web3 debug\_traceCall method is also accessible through Web3 libraries for Ethereum, enabling seamless debugging workflows for decentralized application development.

\\
