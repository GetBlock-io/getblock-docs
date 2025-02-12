---
description: >-
  The eth_getBlockReceipts method is part of the Ethereum JSON RPC Core API and
  is used to retrieve all transaction receipts for a given block. A transaction
  receipt contains essential information.
---

# eth\_getBlockReceipts - Ethereum

{% hint style="success" %}
Returns all transaction receipts for a given block.
{% endhint %}

The eth\_getBlockReceipts method is part of the Ethereum JSON RPC Core API and is designed for retrieving transaction receipts from a specified block. This method provides all transaction receipts within a single block, enabling developers to analyze transaction outcomes in bulk.

### Supported Networks

The eth\_getBlockReceipts RPC Ethereum method is available across all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

The eth\_getBlockReceipts method accepts the following parameter:

* DATA, 32 Bytes: (None) The block number in hexadecimal format or one of the string tags (latest, earliest, or pending).

### Request Example

URL (API Endpoints)\
https://go.getblock.io/\<ACCESS-TOKEN>/

To use this method, send a JSON object containing the jsonrpc, method, and params fields. Below is an example using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": [
        "0xacffc1"
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
"method": "eth_getBlockReceipts",
"params": ["0xacffc1"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response Example

The server responds with a JSON object. Below is an example response for the eth\_getBlockReceipts method:

```json
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

* result: The transaction receipts of the specified block. null if the method is not allowed or unavailable.
* status\_code: The HTTP status code for the request.
* message: A descriptive message explaining the response, e.g., "Method not allowed."

### Returns

When successful, the eth\_getBlockReceipts method returns all transaction receipts for a given block. Each transaction receipt includes details such as:

* transaction: The transaction hash and execution results.
* block: The block containing the transaction.
* parameters: Metadata and execution parameters associated with the receipts.
* value: The value transferred in transactions, encoded as hexadecimal.

### Use Case

The eth\_getBlockReceipts method is particularly useful for developers analyzing multiple transaction outcomes in a specific block. By retrieving all receipts in bulk, this method enables efficient debugging and transaction auditing. If a eth\_getBlockReceipts error occurs, ensure the block number or tag provided is valid and that the node supports this method. Refer to the eth\_getBlockReceipts example for constructing a correct request.

### Example Code

Below is an example of how to call the eth\_getBlockReceipts method programmatically using Python:

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
    "method": "eth_getBlockReceipts",
    "params": [
        "0xacffc1"
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

{% tab title="JavaScript " %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/'; 
const headers = {
    'Content-Type': 'application/json'
};
const data = {
    jsonrpc: "2.0",
    method: "eth_getBlockReceipts",
    params: [
        "0xacffc1"     ],
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

This Python script demonstrates how to interact with the eth\_getBlockReceipts method programmatically. Replace \<ACCESS-TOKEN> with your actual API key. The Web3 eth\_getBlockReceipts method can also be utilized via Web3 libraries for Ethereum.
