---
description: >-
  Retrieve the number of transactions in a specific Ethereum block by its block
  number using eth_getBlockTransactionCountByNumber. Useful for real-time data
  and historical block activity analysis.
---

# eth\_getBlockTransactionCountByNumber - Ethereum

{% hint style="success" %}
The eth\_getBlockTransactionCountByNumber method is a part of the Ethereum Core API that allows users to retrieve the number of transactions in a specific block using its block number.
{% endhint %}

By leveraging the JSON RPC protocol, this method facilitates requests to query the Ethereum blockchain, providing insights into the volume of transactions within a given block. This is especially useful for applications requiring real-time data or historical analysis of block activity.

### Supported Networks

The eth\_getBlockTransactionCountByNumber RPC Ethereum method works on the following Ethereum network types:

* Mainnet
* Testnet: Sepolia, Holesky

This method is compatible across multiple Ethereum networks, ensuring that developers can request transaction counts on both the mainnet and various testnets.

### Parameters

The eth\_getBlockTransactionCountByNumber method accepts a single parameter:

* QUANTITY | TAG: This parameter can be an integer representing the block number or a string representing one of the special tags (latest, earliest, or pending). It specifies which block to query:
*
  * latest: The most recent mined block.
  * earliest: The first block in the chain (genesis block).
  * pending: The next block expected to be mined.

This flexibility allows users to request the transaction count for a specific block number or a dynamic block based on its state in the chain.

### Request Example

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here is an example request using JSON RPC to fetch the number of transactions in a specific block

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_getBlockTransactionCountByNumber",
    "params": [
        "0x52A8CA"
    ],
    "id": "getblock.io"
}
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByNumber",
"params": ["0x52A8CA"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

In this example, we are querying the block with the hexadecimal number "0x52A8CA". This request asks for the total number of transactions in the specified block.

### Response Example

The response from the eth\_getBlockTransactionCountByNumber method will look like this:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x49"
}
```

### Response Description

The eth\_getBlockTransactionCountByNumber method returns the value of the transaction count in hexadecimal format. The response fields include:

* id: The unique identifier for the request.
* jsonrpc: The version of the JSON RPC protocol used.
* result: The number of transactions in the block, represented as a hexadecimal value.

For example, "result": "0x49" signifies that there are 73 transactions within the specified block.

### Use Case

The eth\_getBlockTransactionCountByNumber method is commonly used in scenarios where an application needs to determine the transaction volume of a particular block. It is useful for building analytics tools, blockchain explorers, and monitoring systems that need to provide real-time or historical data on the number of transactions per block.

For instance, a blockchain explorer might use this method to display the transaction count for each block on its overview page, or a monitoring system could use it to identify blocks with unusually high transaction counts.

### Code Example

Here’s how you can use Web3 eth\_getBlockTransactionCountByNumber in a JavaScript environment to retrieve the transaction count of a specific block number:

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
    "method": "eth_getBlockTransactionCountByNumber",
    "params": [
        "0x52A8CA"
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

// Replace <ACCESS-TOKEN> with your actual API key
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

// Request payload
const requestBody = {
  jsonrpc: '2.0',
  method: 'eth_getBlockTransactionCountByNumber',
  params: ['0x52A8CA'], // Replace with the block number in hexadecimal
  id: 'getblock.io',
};

// Axios POST request
axios
  .post(url, requestBody, {
    headers: {
      'Content-Type': 'application/json',
    },
  })
  .then((response) => {
    const transactionCount = response.data.result;
    console.log(`Transaction Count: ${transactionCount}`);
  })
  .catch((error) => {
    console.error('Error fetching transaction count:', error.message);
  });

```
{% endtab %}
{% endtabs %}

This Web3 eth\_getBlockTransactionCountByNumber example demonstrates how to fetch the transaction count for a given block number using the Web3 library. The code handles errors gracefully by logging them, including cases where the eth\_getBlockTransactionCountByNumber error might occur if the block number is invalid.

### Common Errors

1. eth\_getBlockTransactionCountByNumber error - This error may occur if the provided block number is invalid or out of range.
2. Invalid block number - If the block number is not a valid hexadecimal or integer, or it refers to a block that has not been mined yet, the JSON RPC server will return an error.
3. Network issues - If there are connectivity problems with the Core API or the JSON RPC endpoint, you may experience timeouts or other errors during the request.
