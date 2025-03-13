---
description: >-
  The eth_newPendingTransactionFilter method creates a filter in the node to
  notify when new pending transactions arrive.
---

# eth\_newPendingTransactionFilter-Ethereum

{% hint style="success" %}
Creates a filter in the node, to notify when new pending transactionsarrive. To check if the state has changed, call eth\_getFilterChanges.
{% endhint %}

The eth\_newPendingTransactionFilter method is part of the Ethereum JSON-RPC API and is used to create a filter in the node to notify when new pending transactions arrive. This method is essential for monitoring transaction activity in real time. To check for updates, the eth\_getFilterChanges method can be called.

### Supported Networks

The eth\_newPendingTransactionFilter RPC Ethereum method works across various Ethereum network types, including:

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

To interact with the Ethereum eth\_newPendingTransactionFilter endpoints using JSON-RPC, use the following examples

{% tabs %}
{% tab title="JSON" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newPendingTransactionFilter",
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
"method": "eth_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response contains the ID of the newly created filter, which can be used to query changes.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x55c90c9069cac4e02c8868aa6e437dbb"
}
```

### Response Description

* result: A string containing the filter ID in hexadecimal format. This value can be used with methods such as eth\_getFilterChanges or eth\_uninstallFilter to manage and query the filter.

### Use Case

The eth\_newPendingTransactionFilter RPC Ethereum method is widely used in decentralized applications (DApps) and monitoring tools to:

* Track pending transactions in real time.
* Monitor network activity for transaction patterns or anomalies.
* Provide dynamic notifications to users about transactions in progress.

For instance, a Web3 application may use the Ethereum eth\_newPendingTransactionFilter method to notify users about their pending transactions and update the application state based on transaction status.

### Code Example

Here is an eth\_newPendingTransactionFilter example of how to query the method using Python and JavaScript:

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
    "method": "eth_newPendingTransactionFilter",
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
  method: 'eth_newPendingTransactionFilter',
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

#### Common Errors

When using the eth\_newPendingTransactionFilter RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* Network Connectivity Problems: Verify that the network being queried is reachable and the correct endpoint is being used.
* eth\_newPendingTransactionFilter error: This error may occur if the node does not support filtering or if the request is malformed.

By integrating the Web3 eth\_newPendingTransactionFilter method into your application, you can efficiently track transaction activity. Use this core API method to monitor pending transactions and provide a responsive user experience in real time.
