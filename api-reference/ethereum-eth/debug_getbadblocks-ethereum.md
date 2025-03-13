---
description: >-
  The debug_getBadBlocks method is part of the Ethereum JSON RPC Core API and
  returns a list of invalid blocks observed by the Ethereum client.
---

# debug\_getBadBlocks - Ethereum

{% hint style="success" %}
Returns a list of invalid blocks. This is used to detect and analyzeconsensus flaws.
{% endhint %}

The debug\_getBadBlocks method is part of the Ethereum JSON RPC Core API, designed to return a list of invalid blocks observed by the Ethereum client. This method is particularly useful for developers and network operators to detect and analyze consensus flaws.

### Supported Networks

The debug\_getBadBlocks RPC Ethereum method supports all Ethereum network types, including:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters.
{% endhint %}

### Request

URL (API Endpoints)\
https://go.getblock.io/\<ACCESS-TOKEN>/

To make a request, send a JSON object with the jsonrpc, method, and params fields. Below is an example of how to make a request using curl:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \  
--header 'Content-Type: application/json' \  
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_getBadBlocks",
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
"method": "debug_getBadBlocks",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing information about invalid blocks. Below is an example of a typical response:

```
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

The response contains details about invalid blocks, including:

* result: A list of bad blocks or null if none are found.
* status\_code: The HTTP status code of the response.
* message: An explanatory message, such as "Method not allowed."

Each invalid block object in the response includes the following fields:

* hash: The hash of the block.
* block: A detailed block object or null if not found.
* baseFeePerGas: The base fee per gas in the block.
* difficulty: The difficulty level of the block.
* extraData: Additional data included in the block.
* gasLimit: The maximum gas allowed in the block.
* gasUsed: The total gas used by all transactions in the block.
* miner: The address of the miner who mined the block.
* transactions: An array of transaction objects, including fields like transaction, blockHash, blockNumber, from, to, value, and gas.

### Use Case

The debug\_getBadBlocks method is critical for monitoring and debugging the Ethereum network. By identifying invalid blocks, developers can analyze potential consensus issues and ensure the robustness of their node setups. If a debug\_getBadBlocks error is encountered, ensure that the Ethereum client supports this method and is correctly configured. An included debug\_getBadBlocks example demonstrates the proper usage of this method.

### Example Code

You can also make requests to the debug\_getBadBlocks method programmatically using Python. Below is an example using the requests library:

{% tabs %}
{% tab title="Pyhton" %}
```python
import requests
import json


# Define the API URL and access token
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}


# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "debug_getBadBlocks",
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
const headers = {
    'Content-Type': 'application/json'
};
const data = {
    jsonrpc: "2.0",
    method: "debug_getBadBlocks",
    params: [],
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

This Python script sends a request to the debug\_getBadBlocks method and prints the returned information. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 debug\_getBadBlocks method can also be accessed via Web3 libraries for Ethereum, offering a convenient way to retrieve invalid block information programmatically.

The Ethereum debug\_getBadBlocks method serves as a vital tool for identifying and analyzing invalid blocks in the Ethereum network. As part of the Core API Endpoints, this method provides transparency and insights necessary for maintaining network integrity and resolving consensus issues.

\\
