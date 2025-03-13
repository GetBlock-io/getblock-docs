---
description: >-
  The debug_accountRange method is part of the Ethereum JSON RPC Core API and
  retrieves a range of accounts stored in the state trie of the Ethereum node
  for debugging purposes.
---

# debug\_accountRange - Ethereum

{% hint style="success" %}
The debug\_accountRange method retrieves a range of accounts stored in the state trie of the Ethereum node, along with their details, for debugging purposes.
{% endhint %}

The debug\_accountRange method is part of the Ethereum JSON RPC Core API. It is used to enumerate all accounts at a given block with paging capability. The method allows developers to retrieve a specified number of accounts (maxResults) starting from a particular hashed address (start key). If the incompletes parameter is set to false, accounts lacking a key preimage (the actual address) in the database are skipped. Note that Geth, by default, does not store preimages, which may affect the results.

### Supported Networks

The debug\_accountRange RPC Ethereum method supports the following Ethereum network types:

* Mainnet
* Testnets: Sepolia, Holesky

### Parameters

The debug\_accountRange method accepts the following parameters:

* blockNrOrHash (DATA): The block number or hash.
* start (DATA): The hashed address from which to start the enumeration.
* maxResults (DATA): The maximum number of accounts to return per page.
* incompletes (DATA): A boolean value. If set to false, accounts without a key preimage are skipped.

### Request

URL (API Endpoint)

```
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
    "method": "debug_accountRange",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x0f",
        0,
        false,
        false,
        false
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
"method": "debug_accountRange",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7", "0x0f", 0, false, false, false],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The server responds with a JSON object indicating whether the method was allowed or denied. Below is an example of a typical response when the method is not allowed:

```json
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```

### Response Description

* result: The result of the method call. In this case, null is returned when the method is not allowed.
* status\_code: The HTTP status code. A 405 status indicates that the method is not allowed.
* message: A descriptive message about the status of the request.

### Use Case

The debug\_accountRange method can be used for detailed account enumeration in specific blockchain scenarios where paging capabilities are needed. Developers can use this method to retrieve accounts starting from a particular hashed address and define how many results should be returned per request. The debug\_accountRange error message, such as "Method not allowed," indicates that the node does not support the method or the request parameters are incorrect. An illustrative debug\_accountRange example is included in this documentation to demonstrate proper usage.

## Code Example

Below is an example of how to call the debug\_accountRange method programmatically using Python:

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
    "method": "debug_accountRange",
    "params": [
        "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
        "0x0f",
        0,
        False,
        False,
        False
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
  method: 'debug_accountRange',
  params: [
    "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7",
    "0x0f",
    0,
    false,
    false,
    false
  ],
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

This Python script sends a request to the debug\_accountRange method and prints the response. Replace \<ACCESS-TOKEN> with your actual API token. The Web3 debug\_accountRange method can also be accessed through Web3 libraries for Ethereum, providing developers with flexibility and programmatic access.

The Ethereum debug\_accountRange method is part of the Core API and provides critical functionality for enumerating accounts at specific blocks with paging capabilities. As one of the Core API Endpoints, this method is instrumental for developers needing detailed account data on the Ethereum blockchain.
