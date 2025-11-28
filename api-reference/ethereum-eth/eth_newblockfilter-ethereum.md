---
description: >-
  The eth_newBlockFilter method is part of the Ethereum JSON RPC API and is used
  to create a filter in the node that notifies when a new block is added to the
  blockchain.
---

# eth\_newBlockFilter-Ethereum

{% hint style="success" %}
Creates a filter in the node, to notify when a new block arrives. Tocheck if the state has changed, call eth\_getFilterChanges.
{% endhint %}

The eth\_newBlockFilter method is part of the Ethereum JSON RPC API and is used to create a filter in the node that notifies when a new block is added to the blockchain. This method is essential for monitoring block updates and is frequently utilized in Web3 applications to track real-time blockchain activity. To check for changes, the filter can be queried using the eth\_getFilterChanges method.

### Supported Networks

The eth\_newBlockFilter RPC Ethereum method works on various Ethereum network types, including:

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
This method does not require any parameters. The request can be sent with an empty parameters array.
{% endhint %}

### Request

URL

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_newBlockFilter endpoint using JSON-RPC, use the following examples

{% tabs %}
{% tab title="JSON" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newBlockFilter",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="WS" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_newBlockFilter",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response contains the ID of the newly created filter, which can be used to query for changes.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xec7fd9e32b6a15314c8d4ebfa2d3daa9"
}
```

### Response Description

* result: A string containing the filter ID in hexadecimal format. This ID is used with the eth\_getFilterChanges or eth\_uninstallFilter methods.

### Use Case

**The value returned by the eth\_newBlockFilter method, which is the filter ID, is a crucial part of real-time blockchain monitoring. Applications can use this value to retrieve updates and check for changes effectively.**

The eth\_newBlockFilter RPC Ethereum method is commonly used in decentralized applications (DApps) and monitoring tools to:

* Track new block arrivals in real time.
* Trigger specific actions when a block is added.
* Monitor blockchain activity for events that rely on new blocks.

For example, a Web3 application may use the Ethereum eth\_newBlockFilter method to notify users of incoming transactions or to update the application state with the latest block data

### Code Example

Here is an eth\_newBlockFilter example of how to query the method using Python and JavaScript:

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
    "method": "eth_newBlockFilter",
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
import axios from 'axios';

// Define the API URL and headers
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_newBlockFilter',
  params: [],
  id: 'getblock.io'
};

// Send the POST request
axios.post(url, data, { headers })
  .then(response => {
    // Print the result
    console.log('Filter ID:', response.data.result);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

#### **Common Errors**

When using the eth\_newBlockFilter RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure that the URL and token are correct and active.
* Network Connectivity Problems: Verify that the network being queried is reachable and the correct endpoint is being used.
* eth\_newBlockFilter error: This error may occur if the node does not support filters or if the request is malformed.

By integrating the Web3 eth\_newBlockFilter method into your application, you can monitor blockchain activity efficiently. Use this core API method to create filters and track new blocks in real time, enhancing the responsiveness of your DApp or monitoring tools.
