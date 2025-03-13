---
description: >-
  The eth_submitHashrate method is part of the Ethereum JSON-RPC API and is used
  to submit the mining hashrate. It is commonly used by mining software, such as
  Ethminer, to report hash rates.
---

# eth\_submitHashrate Ethereum

{% hint style="success" %}
Submits the mining hashrate.Used by mining software such as Ethminer.
{% endhint %}

The eth\_submitHashrate method is part of the Ethereum JSON-RPC API and is used to submit the mining hashrate. This method is commonly used by mining software, such as Ethminer, to report their hash rates to the Ethereum network.

### Supported Networks

The eth\_submitHashrate RPC Ethereum method is supported on the following network types:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

The method accepts two parameters:

1. DATA (32 Bytes): A hexadecimal string representation of the hash rate.
2. DATA (32 Bytes): A random hexadecimal ID identifying the client submitting the hashrate.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_submitHashrate endpoint using JSON-RPC, use the following examples:

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_submitHashrate",
    "params": [
        "0x500000",
        "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"
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
"method": "eth_submitHashrate",
"params": ["0x500000", "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response indicates whether the hashrate submission was successful.

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": true
}
```

### Response Description

* result: A boolean value indicating the success (true) or failure (false) of the hashrate submission.
* value: Not directly included in the response, but the value of the submitted hashrate is specified in the parameters.

### Use Case

The eth\_submitHashrate RPC Ethereum method is widely used by mining software and monitoring tools to:

* Report mining hashrate to the Ethereum network.
* Provide visibility into the computational power contributed by individual miners or mining pools.
* Ensure accurate representation of mining activity on the blockchain.

For instance, a Web3 application that integrates mining functionalities may use the Ethereum eth\_submitHashrate method to send hashrate data for analytics or reporting.

### Code Example

Here is an eth\_submitHashrate example of how to query the method using Python and JavaScript:

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
    "method": "eth_submitHashrate",
    "params": [
        "0x500000",
        "0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c"
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
  method: 'eth_submitHashrate',
  params: [
    '0x500000',
    '0x59daa26581d0acd1fce254fb7e85952f4c09d0915afd33d3886cd914bc7d283c'
  ],
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

**Common Errors**

When using the eth\_submitHashrate RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* eth\_submitHashrate error: This could happen if the parameters are improperly formatted.
* Incorrect Hashrate or Client ID: Verify that the submitted hexadecimal strings are valid and meet the 32-byte requirement.

By integrating the Web3 eth\_submitHashrate method into your application, you can provide accurate hashrate reporting and ensure proper representation of mining contributions. Use this core API method to monitor and report mining activity seamlessly.
