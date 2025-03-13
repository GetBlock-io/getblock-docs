---
description: >-
  Retrieve logs matching a specified filter using eth_getLogs. Essential for
  monitoring blockchain events, smart contract interactions, and building
  event-driven functionality in Ethereum dApps.
---

# eth\_getLogs - Ethereum

{% hint style="success" %}
This method returns an array of logs that match a specified filter object, making it a powerful tool for retrieving on-chain event information related to specific addresses or topics.
{% endhint %}

Ethereum eth\_getLogs JSON-RPC Core API, used to interact with Ethereum nodes. Logs are often used to monitor the blockchain for events emitted by smart contracts, which is crucial for DApp developers building event-driven functionality.

To optimize the retrieval of logs, it's recommended to leave the --auto-log-bloom-caching-enabled command line option at its default value of true. This enhances log retrieval performance and ensures efficient blockchain data access. Note that the eth\_getLogs limit may apply depending on the node configuration.

### Supported Networks

The eth\_getLogs RPC Ethereum method supports the following network types

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* Object: A filter options object, which includes the following fields:
  * fromBlock (optional): Block number from which to start looking for logs.
  * toBlock (optional): Block number to stop looking for logs.
  * address (optional): The contract address from which to get logs.
  * topics (optional): Array of topic filters.

### Request

URL (API Endpoint)

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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x107D7B0",
            "toBlock": "0x107D7B0",
            "address": "0x901c7C311d39e0b26257219765E71E8DB3107A81",
            "topics": []
        }
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="WS" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"fromBlock": "0x107D7B0", "toBlock": "0x107D7B0", "address": "0x901c7C311d39e0b26257219765E71E8DB3107A81", "topics": []}],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object containing the array of logs that match the given filter. Below is an example of a typical response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": []
}
```

### Response Description

* result: An array of logs that match the specified filter, represented as objects. If no logs are found, the result will be an empty array.

### Use Case

The eth\_getLogs method is particularly useful for DApp developers who need to monitor the blockchain for specific events related to their smart contracts. By specifying parameters such as block range and contract address, developers can efficiently retrieve log data for their applications. This is often needed for building functionalities such as notifications, analytics, or data-driven user interfaces. In case of an eth\_getLogs error, developers can adjust the filter parameters to ensure they are not exceeding any limits set by the node.

### Code Example

You can also make requests to the eth\_getLogs method programmatically using different programming languages.Below is an example :

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
    "method": "eth_getLogs",
    "params": [
        {
            "fromBlock": "0x107D7B0",
            "toBlock": "0x107D7B0",
            "address": "0x901c7C311d39e0b26257219765E71E8DB3107A81",
            "topics": []
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

// Define the API URL and access token
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_getCode',
  params: [
    '0xa50a51c09a5c451c52bb714527e1974b686d8e77',
    'latest'
  ],
  id: 'getblock.io'
};

// Send the POST request
axios.post(url, data, { headers })
  .then(response => {
    // Print the result
    console.log(JSON.stringify(response.data, null, 4));
  })
  .catch(error => {
    console.error('Error:', error);
  });

```
{% endtab %}
{% endtabs %}

This is a Web3 eth\_getLogs example for using the method in , providing an interface to access blockchain data for various use cases, including transaction tracking and event monitoring.
