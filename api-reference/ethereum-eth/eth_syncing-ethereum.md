---
description: >-
  The eth_syncing method is part of the Ethereum JSON-RPC API and is used to
  check the synchronization status of an Ethereum node. It returns detailed
  synchronization data if the node is syncing
---

# eth\_syncing-Ethereum

{% hint style="success" %}
Returns an object with data about the synchronization status, or falseif not synchronizing.
{% endhint %}

The eth\_syncing method is part of the Ethereum JSON-RPC API and is used to check the synchronization status of an Ethereum node. This method returns detailed synchronization data if the node is syncing, or false if the node is fully synchronized.

### Supported Networks

The eth\_syncing RPC Ethereum method works on the following network types:

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

To interact with the Ethereum eth\_syncing endpoint using JSON-RPC, use the following examples:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_syncing",
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
"method": "eth_syncing",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response indicates whether the node is synchronizing. If the node is synchronizing, an object with detailed synchronization status is returned. If the node is fully synchronized, false is returned.

Example Response

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```

Example Response (Node Synchronizing)

```
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "startingBlock": "0x384",
        "currentBlock": "0x386",
        "highestBlock": "0x454"
    }
}
```

**Response Description**

* result: The synchronization status:
  * false: The node is fully synchronized.
  * Object: Detailed data about synchronization status, including:
    * startingBlock: The block at which the node started syncing.
    * currentBlock: The block number the node has currently synced to.
    * highestBlock: The highest block the node is aware of.
* value: Represents the synchronization progress, implicitly indicated by the currentBlock and highestBlock values.

### Use Case

The eth\_syncing RPC Ethereum method is widely used in monitoring and analytics tools to:

* Determine the synchronization status of a node.
* Track progress during node setup or recovery.
* Provide insights into blockchain synchronization behavior.

For example, a Web3 monitoring application may use the Ethereum eth\_syncing method to notify users when their node has completed synchronization or to display real-time sync progress.

### Code Example

Here is an eth\_syncing example of how to query the method using Python and JavaScript:

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
    "method": "eth_syncing",
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
  method: 'eth_syncing',
  params: [],
  id: 'getblock.io'
};

// Send the POST request
axios.post(url, data, { headers })
  .then(response => {
    // Print the result
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });

```
{% endtab %}
{% endtabs %}

#### Common Errors

When using the eth\_syncing RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* eth\_syncing error: This could happen if the node is unreachable or the request format is incorrect.
* Unexpected Response: Verify that the node supports synchronization queries and is actively syncing if expected.

By integrating the Web3 eth\_syncing method into your application, you can monitor synchronization status efficiently. Use this core API method to ensure your node is up-to-date and ready for blockchain operations.
