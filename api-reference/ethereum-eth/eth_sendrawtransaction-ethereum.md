---
description: >-
  The eth_sendRawTransaction method is part of the Ethereum JSON RPC API and is
  used to send a signed transaction to the network. It can transfer Ether,
  deploy a smart contract, or interact .
---

# eth\_sendRawTransaction-Ethereum

{% hint style="success" %}
Sends a signed transaction. A transaction can send ether, deploy acontract, or interact with a contract
{% endhint %}

The eth\_sendRawTransaction method is part of the Ethereum JSON RPC API and is used to send a signed transaction to the network. Transactions sent using this method can transfer Ether, deploy a smart contract, or interact with an existing contract. This method is widely used in Web3 applications to execute blockchain transactions securely.

To avoid exposing your private key, it is recommended to create signed transactions offline and use the eth\_sendRawTransaction RPC Ethereum method to send the serialized transaction data.

### Supported Networks

The eth\_sendRawTransaction RPC Ethereum method works across various Ethereum network types, including:

* Mainnet
* Testnet: Sepolia, Holesky

### Parameters

The method accepts a single parameter:

* data: The signed transaction serialized into hexadecimal format.

### Request

URL (API Endpoint)

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

To interact with the Ethereum eth\_sendRawTransaction endpoint using JSON-RPC, use the following examples

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="ws" %}
```json
wscat -c wss://eth.getblock.io/YOUR-API-KEY/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_sendRawTransaction",
"params": ["0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

### Response

The response may indicate success or an error, depending on the validity of the transaction.

Example Response (Error)

```json
{
    "error": {
        "code": -32000,
        "message": "only replay-protected (EIP-155) transactions allowed over RPC"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```

### Response Description

* error: Contains the error details if the transaction was invalid. For example, "only replay-protected (EIP-155) transactions allowed over RPC".
* result: The transaction hash if the transaction is successfully broadcast to the network.
* value: Not directly included in the response, but the value sent in the transaction is specified in the serialized data.

### Use Case

The eth\_sendRawTransaction RPC Ethereum method is commonly used in decentralized applications (DApps) and wallet solutions to:

* Deploy new smart contracts.
* Interact with existing contracts by sending signed transactions.
* Execute secure Ether transfers without exposing private keys.

For instance, a Web3 application may use the Ethereum eth\_sendRawTransaction method to enable users to send transactions directly from their wallets.

### Code Example

Here is an eth\_sendRawTransaction example of how to query the method using Python and JavaScript:

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
    "method": "eth_sendRawTransaction",
    "params": [
        "0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"
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
  method: 'eth_sendRawTransaction',
  params: [
    '0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833'
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

#### Common Errors

When using the eth\_sendRawTransaction RPC Ethereum method, the following issues may occur:

* Invalid URL or ACCESS-TOKEN: Ensure the URL and token are correct and active.
* eth\_sendRawTransaction error: This could happen if the transaction is improperly signed or formatted.
* eth\_sendrawtransaction invalid sender: Indicates that the sender's address is invalid or does not match the signature.
* Replay Protection: Only replay-protected (EIP-155) transactions are allowed over RPC.

By integrating the Web3 eth\_sendRawTransaction method into your application, you can securely send transactions and interact with the Ethereum blockchain. Use this core API method to enable seamless user experiences while maintaining high security.

\\
