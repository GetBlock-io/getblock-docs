---
description: >-
  eth_sendRawTransaction method in the JSON-RPC API Interface allows users to
  send raw transactions on the BSC protocol efficiently.
---

# eth\_sendRawTransaction - Binance Smart Chain

{% hint style="success" %}
It submits a signed transaction to the Binance Smart Chain network for execution, enabling direct interaction without revealing private keys.
{% endhint %}

The `eth_sendRawTransaction` method in the BSC protocol is a crucial feature of the `eth_sendRawTransaction Web3` interface. It enables users to broadcast a signed transaction to the network without requiring local signing. This method directly pushes the raw transaction data to the blockchain, ensuring efficient processing.

As part of the `eth_sendRawTransaction RPC protocol`, this method requires a single parameter: the raw, hex-encoded transaction data. Upon successful execution, it returns the transaction hash, allowing users to track its status. This approach enhances security by keeping private keys off the client-side, making it a preferred choice for decentralized applications.

### Supported Networks

The eth\_sendRawTransaction JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_sendRawTransaction` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **signed transaction**
  * **Type**: String
  * **Description**: The raw signed transaction data encoded as a hexadecimal string. This is the serialized and signed transaction that you want to broadcast to the Ethereum network.
  * **Required**: Yes
  * **Default/Supported Values**: Must be a valid signed transaction in hexadecimal format.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_sendRawTransaction :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_sendRawTransaction",
  "params": ["signed transaction"],
  "id": 1
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_sendRawTransaction upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "invalid argument 0: json: cannot unmarshal hex string without 0x prefix into Go value of type hexutil.Bytes"
  }
}

```

### Body Parameters

Here is the list of body parameters for the `eth_sendRawTransaction` method:

1. **jsonrpc**: A string specifying the version of the JSON-RPC protocol. Typically, this is set to `"2.0"`.
2. **id**: A unique identifier for the request. It can be a number, string, or null. This is used to match the response with the request.
3. **error**: An object containing error details if the request fails. The object includes:
   * **code**: A numeric code representing the error type. In this case, `-32602` indicates an invalid parameter.
   * **message**: A string description of the error. For instance, "invalid argument 0: json: cannot unmarshal hex string without 0x prefix into Go value of type hexutil.Bytes" indicates that the input was expected to be a hex string prefixed with `0x`.

### Use Cases

Here are some use-cases for `eth_sendRawTransaction` method:

1. **Submitting Transactions with Higher Security**: The `eth_sendRawTransaction` method is commonly used in scenarios where security is a priority. By signing the transaction locally, typically on a user's device, the private key never leaves the user's environment. This reduces the risk of exposing sensitive information to potentially insecure networks or third-party services.
2. **Interacting with Smart Contracts**: Developers use `eth_sendRawTransaction` to interact with smart contracts on the Ethereum blockchain. By crafting and signing a transaction that calls a smart contract function, developers can execute complex operations such as token transfers, decentralized finance (DeFi) operations, or any other smart contract logic.
3. **Enabling Offline Transaction Signing**: This method is ideal for creating offline transaction workflows. Users can sign transactions offline and then broadcast them later using `eth_sendRawTransaction`. This is particularly useful in environments where internet connectivity is intermittent or when users want to prepare transactions in advance and submit them when they have access to the network.

### Code for eth\_sendRawTransaction

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "invalid argument 0: json: cannot unmarshal hex string without 0x prefix into Go value of type hexutil.Bytes"
  }
}

response = requests.post(url, headers=headers, data=json.dumps(payload))

# Check the response and print the result
if response.status_code == 200:
    print("Result:", response.json().get("result"))
else:
    print("Error:", response.status_code, response.text)

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "invalid argument 0: json: cannot unmarshal hex string without 0x prefix into Go value of type hexutil.Bytes"
  }
};

axios.post(url, payload, {
  headers: { "Content-Type": "application/json" }
})
.then(response => {
  console.log("Result:", response.data.result);
})
.catch(error => {
  if (error.response) {
    console.error("Error:", error.response.status, error.response.data);
  } else {
    console.error("Request failed:", error.message);
  }
});
```
{% endtab %}
{% endtabs %}

### Common Errors

When using the `eth_sendRawTransaction` JSON-RPC API BSC method, the following issues may occur:

* **Invalid Signature**: If the transaction signature is incorrect, the transaction will be rejected. Ensure that the private key used to sign the transaction is correct and that the signing process is properly implemented.
* **Insufficient Funds**: Transactions may fail if the sending account does not have enough BNB to cover the gas fees. Verify the account balance before sending a transaction to prevent this error.
* **Nonce Too Low**: If the nonce of the transaction is lower than expected, it will be rejected. Keep track of the nonce for each transaction sent from the account to avoid this issue.
* **Gas Limit Exceeded**: If the transaction's gas limit is set too low, it will not be processed. Estimate the gas required for the transaction accurately and set an appropriate gas limit.

The `eth_sendRawTransaction` method is highly beneficial in Web3 applications as it allows for the submission of pre-signed transactions, enabling users to interact with the blockchain without exposing their private keys. This enhances security and efficiency, as transactions can be prepared offline and submitted directly to the network.

### Conclusion

The method `eth_sendRawTransaction` is a crucial component of the JSON-RPC API, enabling users to broadcast signed transactions directly to the Ethereum network or compatible networks like BSC. By using `eth_sendRawTransaction`, developers can ensure that their transactions are securely and efficiently processed on the blockchain, leveraging the power of decentralized networks.
