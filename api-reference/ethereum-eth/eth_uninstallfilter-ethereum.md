---
description: >-
  The eth_uninstallFilter method is part of the Ethereum JSON-RPC Core API and
  is used to uninstall a filter with a specified filter ID. It is crucial for
  managing filters that are no longer required.
---

# eth\_uninstallFilter - Ethereum

{% hint style="success" %}
This method uninstalls a filter with the specified filter ID
{% endhint %}

The eth\_uninstallFilter method is part of the Ethereum JSON RPC Core API, used to interact with Ethereum nodes. The eth\_uninstallFilter RPC Ethereum method is crucial for managing filters that are no longer required. Filters are automatically timed out if they are not queried using eth\_getFilterChanges or eth\_getFilterLogs for 10 minutes.

### Supported Networks

The eth\_uninstallFilter RPC Ethereum method supports the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

* DATA: The filter ID to uninstall.
* parameters: Additional context or metadata related to the request, if applicable.

### Request

URL (API Endpoint)

```json
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
    "method": "eth_uninstallFilter",
    "params": [
        "0xb"
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
"method": "eth_uninstallFilter",
"params": ["0xb"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object indicating whether the filter was successfully uninstalled. Below is an example of a typical response:

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```

**Response Description**

* result: A boolean value indicating whether the filter was successfully uninstalled. false typically means that the filter ID provided does not exist or has already been removed.

### Use Case

The eth\_uninstallFilter method is particularly useful for applications that manage Ethereum logs or events using filters. Once a filter is no longer needed, calling this method ensures optimal resource usage by uninstalling unnecessary filters. In case of an eth\_uninstallFilter error, developers should verify that the filter ID provided is valid and that the filter has not already timed out. An eth\_uninstallFilter example is provided in this documentation to illustrate proper usage.

### Code Example

You can also make requests to the eth\_uninstallFilter method programmatically using Python. Below is an example using the requests library:

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
    "method": "eth_uninstallFilter",
    "params": [
        "0xb"
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
const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';
const data = {
  jsonrpc: '2.0',
  method: 'eth_uninstallFilter',
  params: ['0xb'],
  id: 'getblock.io',
};

axios.post(url, data, {
  headers: {
    'Content-Type': 'application/json',
  },
})
  .then(response => {
    console.log('Response:', response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```
{% endtab %}
{% endtabs %}

This Python script sends a request to the eth\_uninstallFilter method and prints the result indicating whether the filter was successfully uninstalled. Make sure to replace \<ACCESS-TOKEN> with your actual API token. The Web3 eth\_uninstallFilter method can also be used in Web3 libraries for Ethereum to manage filters programmatically.

The Ethereum eth\_uninstallFilter method provides developers with an efficient way to clean up unused filters. This is particularly important for applications that frequently interact with Ethereum filters and events, ensuring optimal use of resources within the Ethereum JSON RPC API and Core API Endpoints. Transaction management is also essential for optimizing resource usage and API performance.
