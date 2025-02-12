---
description: >-
  The eth_mining method checks if the client is actively mining new blocks. It
  is useful for monitoring mining status in Ethereum systems and indicates that
  Besu pauses mining during synchronization
---

# eth\_mining Ethereum

{% hint style="success" %}
Whether the client is actively mining new blocks. Besu pauses miningwhile the client synchronizes with the network regardless of command settings or methods called.
{% endhint %}

The eth\_mining method is part of the Ethereum JSON RPC API and determines whether the client is actively mining new blocks. This method is useful for understanding the status of mining operations in Ethereum-based systems. It also highlights that Besu pauses mining while the client synchronizes with the network, regardless of command settings or methods called.

### Supported Networks

The eth\_mining RPC Ethereum method works on various Ethereum network types, including:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

{% hint style="info" %}
This method does not require any parameters. The request can be sent with an empty parameters array.
{% endhint %}

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_mining endpoint using JSON-RPC, use the following examples

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_mining",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_mining",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response indicates whether the client is currently mining new blocks.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```

### Response Description

* result: A boolean value indicating the mining status:
  * true: The client is actively mining.
  * false: The client is not mining (e.g., paused due to synchronization).

### Use Case

The eth\_mining method is used in Ethereum-based decentralized applications (DApps) and monitoring tools to verify whether mining is active on the client. By querying the eth\_mining RPC Ethereum method, developers and administrators can:

* Monitor mining activity in real time.
* Debug issues related to mining pauses during network synchronization.
* Provide insights for mining pools or standalone miners.

For instance, a Web3 application may call the Ethereum eth\_mining method to check the mining status before initiating block-dependent operations.

### Code Example

Here is an eth\_mining example of how to query the method using Python and JavaScript:

{% tabs %}
{% tab title="Python" %}
```python
import requests
import json

# Define the API URL and headers
url = 'https://go.getblock.io/<ACCESS-TOKEN>/'
headers = {'Content-Type': 'application/json'}

# Prepare the request data
data = {
    "jsonrpc": "2.0",
    "method": "eth_mining",
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

// Define the API URL and headers
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_mining',
  params: [],
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

#### Common Errors

When using the eth\_mining RPC Ethereum method, the following errors may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* Network Connectivity Problems: Verify the network is reachable and the correct endpoint is being used.
* eth\_mining error: This may occur if the method is not supported on the selected network or if there are synchronization issues with the client.

By integrating the Web3 eth\_mining method into your applications, you can monitor mining activity efficiently and ensure seamless operation in Ethereum-based systems. Use this core API method to retrieve the mining status of the client and adapt your DApp’s behavior accordingly.

\\
