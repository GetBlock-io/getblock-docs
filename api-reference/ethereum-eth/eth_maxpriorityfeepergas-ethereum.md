---
description: >-
  The eth_maxPriorityFeePerGas method returns the hex value of the priority fee
  needed for a transaction to be included in a block. It helps estimate
  transaction costs in Ethereum-based applications
---

# eth\_maxPriorityFeePerGas - Ethereum

{% hint style="success" %}
Returns the hex value of the priority fee necessary to be included in ablock.
{% endhint %}

The eth\_maxPriorityFeePerGas method is part of the Ethereum JSON RPC API and is designed to return the hexadecimal value of the priority fee required to be included in a block. This method is critical for estimating transaction costs in Ethereum-based applications and is widely used in Web3 integrations.

### Supported Networks

The eth\_maxPriorityFeePerGas RPC Ethereum method works across various Ethereum network types, including

* Mainnet
* Testnet: Sepolia, Hoodi

### Parameters

{% hint style="info" %}
This method does not require any parameters. The request can be sent with an empty parameters array.
{% endhint %}

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_maxPriorityFeePerGas endpoint using JSON-RPC, use the following examples

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw {
    "jsonrpc": "2.0",
    "method": "eth_maxPriorityFeePerGas",
    "params": [],
    "id": "getblock.io"
}

```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_maxPriorityFeePerGas",
"params": [],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response contains the priority fee in hexadecimal format, indicating the minimum fee necessary for a transaction to be included in a block.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5f5e100"
}
```

### Response Description

* result: The hexadecimal representation of the priority fee. In the example above, 0x5f5e100 translates to 100,000,000 Wei in decimal.

### Use Case

The eth\_maxPriorityFeePerGas method is used in Ethereum-based decentralized applications (DApps) to determine the appropriate priority fee for transactions. By querying the eth\_maxPriorityFeePerGas RPC Ethereum method, developers can ensure their transactions are processed efficiently without overpaying for gas fees. This is particularly useful for applications integrating Web3, where transaction cost estimation is crucial.

For instance, a wallet application may call the Ethereum eth\_maxPriorityFeePerGas method to calculate a competitive transaction fee in real time, enhancing the user experience.

### Code Example

Here is an eth\_maxPriorityFeePerGas example of how to query the method using Python and JavaScript:

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
    "method": "eth_maxPriorityFeePerGas",
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
  method: 'eth_maxPriorityFeePerGas',
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

**Common Errors**

While querying the eth\_maxPriorityFeePerGas RPC Ethereum method, developers may encounter some common issues:

* Invalid URL or ACCESS-TOKEN: Ensure that the URL and token are correct and active.
* Network Connectivity Problems: Verify that the network being queried is reachable and the correct endpoint is being used.
* eth\_maxPriorityFeePerGas error: This could occur if the method is not supported on the selected network. Check the supported network types and ensure compatibility.

By integrating the Web3 eth\_maxPriorityFeePerGas method into your applications, you can provide precise and efficient transaction fee estimations, enhancing your DApp's functionality and user experience. Use this core API method to ensure your transactions are included in the block seamlessly.
