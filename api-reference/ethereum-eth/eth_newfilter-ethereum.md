---
description: >-
  The eth_newFilter method creates a filter object based on specified options to
  notify the client when the state changes, such as new logs. It is part of the
  Ethereum JSON RPC API
---

# eth\_newFilter-Ethereum

{% hint style="success" %}
Creates a filter object, based on filter options, to notify when thestate changes (logs)
{% endhint %}

The eth\_newFilter method is part of the Ethereum JSON RPC API and is used to create a filter object based on specified filter options. This method notifies the client when the state changes (logs). To check if the state has changed, the eth\_getFilterChanges method can be used.

### Supported Networks

The eth\_newFilter RPC Ethereum method is supported on the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

The method accepts a filter object with the following keys and their values:

* address (optional): A contract address or a list of addresses from which logs should originate.
* fromBlock (optional, default is latest): A hexadecimal block number or one of the strings latest, earliest, or pending.
* toBlock (optional, default is latest): A hexadecimal block number or one of the strings latest, earliest, or pending.
* topics (optional): An array of 32-byte DATA topics. Topics are order-dependent.

**Specifying Topic Filters**

Topics are order-dependent. A transaction log with topics A, B will match the following topic filters:

* \[]: Matches anything.
* \[A]: Matches A in the first position, and anything after.
* \[null, B]: Matches anything in the first position AND B in the second position, and anything after.
* \[A, B]: Matches A in the first position AND B in the second position, and anything after.
* \[\[A, B], \[A, B]]: Matches (A OR B) in the first position AND (A OR B) in the second position, and anything after.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_newFilter endpoint using JSON-RPC, use the following examples:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_newFilter",
    "params": [
        {
            "topics": [
                "0xb7407a6d74c6472bf99c4c9abe0860dc439469421c42c060639733b2309b05c7"
            ]
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
"method": "eth_newFilter",
"params": [{"topics": ["0xb7407a6d74c6472bf99c4c9abe0860dc439469421c42c060639733b2309b05c7"]}],
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
    "result": "0x73d728c1f165d1886b2fd41cf7478a07"
}
```

### Response Description

* result: A string containing the filter ID in hexadecimal format. This ID is used with methods such as eth\_getFilterChanges or eth\_uninstallFilter to manage and query the filter.

### Use Case

The eth\_newFilter RPC Ethereum method is commonly used in decentralized applications (DApps) and monitoring tools to:

* Track specific events or transactions in real time.
* Monitor logs emitted by a smart contract.
* Create dynamic notifications based on blockchain state changes.

For instance, a Web3 application may call the Ethereumeth\_newFilter method to track specific events emitted by a smart contract and update the UI or trigger custom workflows when those events occur.

### Code Example

Here is an eth\_newFilter example of how to query the method using Python and JavaScript:

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
    "method": "eth_newFilter",
    "params": [
        {
            "topics": [
                "0xb7407a6d74c6472bf99c4c9abe0860dc439469421c42c060639733b2309b05c7"
            ]
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

// Define the API URL and headers
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const headers = { 'Content-Type': 'application/json' };

// Prepare the request data
const data = {
  jsonrpc: '2.0',
  method: 'eth_newFilter',
  params: [
    {
      topics: [
        '0xb7407a6d74c6472bf99c4c9abe0860dc439469421c42c060639733b2309b05c7'
      ]
    }
  ],
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

When using the eth\_newFilter RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure that the URL and token are correct and active.
* Incorrect Parameters: Verify that the filter object is formatted correctly.
* eth\_newFilter error: This error may occur if the node does not support filtering or if the request is malformed.

By integrating the Web3 eth\_newFilter method into your application, you can efficiently monitor blockchain logs and events. Use this core API method to create filters and track specific transactions or state changes in real time, enhancing the responsiveness of your DApp or monitoring tools.
