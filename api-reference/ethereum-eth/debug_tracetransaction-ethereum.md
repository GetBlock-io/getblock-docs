---
description: >-
  This method reruns a specific transaction with the same state as when it was
  originally executed. By doing so, developers can trace the transaction’s
  execution step by step to understand its behavior.
---

# debug\_traceTransaction - Ethereum

{% hint style="success" %}
Remix uses debug\_traceTransaction to implement debugging. Use theDebugger tab in Remix instead of calling debug\_traceTransactiondirectly.Reruns the transaction with the same state as when the transactionexecuted.
{% endhint %}

The debug\_traceTransaction method is part of the Ethereum JSON RPC Core API and is designed for advanced debugging purposes. This method reruns a specific transaction with the exact same state as when it was initially executed, allowing developers to trace the transaction for debugging purposes. It is commonly used by tools like Remix in the Debugger tab.

### Supported Networks

The debug\_traceTransaction RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Hoodi

### Parameters

The debug\_traceTransaction method accepts the following parameters:

* transactionHash: (data)\
  The hash of the transaction to be traced.
* Object: (None)\
  Request options, all of which are optional and default to false.

### Request

URL&#x20;

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
    "method": "debug_traceTransaction",
    "params": [
        "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
        null
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
"method": "debug_traceTransaction",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c", null],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object. Below is an example response for the debug\_traceTransaction method:

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

If successful, the method provides a detailed trace of the specified transaction, including:

* block: The block containing the transaction.
* transaction: The transaction details and its execution trace.
* parameters: The execution context and associated data.
* value: The values used or modified during transaction execution.

### Use Case

The debug\_traceTransaction method is critical for developers needing to analyze the execution of a specific transaction. It provides insights into opcode-level execution, gas usage, and potential failure points in smart contracts. This method is highly useful for debugging failed transactions and investigating unexpected behavior. If a debug\_traceTransaction error occurs, ensure the provided transaction hash is valid and that the node supports this method. The provided debug\_traceTransaction example demonstrates how to construct a proper request.

### Example Code

Here is an example of how to call the debug\_traceTransaction method programmatically using Python:

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
    "method": "debug_traceTransaction",
    "params": [
        "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
        null
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
import axios from 'axios';

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/'; // Замените <ACCESS-TOKEN> на ваш токен доступа
const headers = {
    'Content-Type': 'application/json'
};
const data = {
    jsonrpc: "2.0",
    method: "debug_traceTransaction",
    params: [
        "0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c",
        null
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

This Python script demonstrates how to interact with the debug\_traceTransaction method programmatically. Replace \<ACCESS-TOKEN> with your actual API key. The Web3 debug\_traceTransaction method can also be used through Web3 libraries for Ethereum.
