---
description: >-
  The debug_traceBlockByNumber method is part of the Ethereum JSON RPC Core API
  and is used for advanced debugging. This method provides a full trace of all
  invoked opcodes for all transactions.
---

# debug\_traceBlockByNumber - Ethereum

{% hint style="success" %}
Returns full trace of all invoked opcodes of all transactions includedin the block.
{% endhint %}

The debug\_traceBlockByNumber method is part of the Ethereum JSON RPC Core API and is designed for advanced debugging purposes. This method provides a full trace of all invoked opcodes of all transactions included in a specified block by its number. It is particularly valuable for developers seeking detailed insights into transaction execution at the opcode level.

### Supported Networks

The debug\_traceBlockByNumber RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

The debug\_traceBlockByNumber method accepts the following parameters:

* quantity|tag: (None) Integer representing a block number or one of the string tags latest, earliest, or pending as described in Block Parameter.
* Object: (Object) Request options, all of which are optional and default to false.

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
    "method": "debug_traceBlockByNumber",
    "params": [
        "0xA1",
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
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "debug_traceBlockByNumber",
"params": ["0xA1", {"tracer": "callTracer"}],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object. Below is an example response for the debug\_traceBlockByNumber method:

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

### Returns

If successful, the method provides a detailed trace of all transactions in the block, including:

* block: The block details, including its number and transactions.
* transaction: A detailed trace of each transaction in the block.
* parameters: Opcode-level execution details and associated data.
* value: The values used or modified during the transaction execution.

### Use Case

The debug\_traceBlockByNumber method is critical for developers needing detailed debugging of Ethereum transactions at the opcode level. It aids in analyzing gas consumption, investigating failed transactions, and diagnosing issues in smart contract execution. If a debug\_traceBlockByNumber error occurs, ensure the provided block number or tag is correct and that your node supports this method. The provided debug\_traceBlockByNumber example demonstrates how to construct a proper request.

### Example Code

Here is an example of how to call the debug\_traceBlockByNumber method programmatically using Python:

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
    "method": "debug_traceBlockByNumber",
    "params": [
        "0xA1",
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
print(json.dumps(response_data, indent=4))
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/'; 
const headers = {
    'Content-Type': 'application/json'
};
const data = {
    jsonrpc: "2.0",
    method: "debug_traceBlockByNumber",
    params: [
        "0xA1", 
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

This Python script demonstrates how to interact with the debug\_traceBlockByNumber method programmatically. Replace \<ACCESS-TOKEN> with your actual API key. The Web3 debug\_traceBlockByNumber method can also be used through Web3 libraries for Ethereum, enabling opcode-level transaction analysis and facilitating advanced debugging scenarios for Ethereum smart contracts and transactions.

\\
