---
description: >-
  eth_sign in the JSON-RPC API Interface allows signing data with a specific
  account's private key on the BSC protocol efficiently.
---

# eth\_sign - Binance Smart Chain

{% hint style="success" %}
The RPC method signs messages with a user's private key on BSC, enabling transaction verification and identity confirmation without exposing the private key.
{% endhint %}

The `eth_sign` method in the BSC protocol is a JSON-RPC API call used to sign messages with an account's private key. This method is integral to the `eth_sign Web3` functionality, allowing developers to authenticate and verify messages securely. By using `eth_sign`, users can ensure the integrity and origin of the message.

In the `eth_sign RPC protocol`, this method requires the address of the account and the message to be signed. The response includes the signed data, which can be used for further verification processes. It's crucial for developers to understand that `eth_sign` does not alter the blockchain state, making it a safe choice for off-chain operations.

### Supported Networks

The eth\_sign JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_sign` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Parameter 1:**
  * **Type:** `string`
  * **Description:** The address of the account to sign with.
  * **Required:** Yes
  * **Example Value:** `"0xcee8ae756461e2653b88aefdbd70c1144de52b23"`
* **Parameter 2:**
  * **Type:** `string`
  * **Description:** The data to sign, which should be a hexadecimal string.
  * **Required:** Yes
  * **Example Value:** `"0xbcda"`

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_sign :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_sign",
"params": ["0xcee8ae756461e2653b88aefdbd70c1144de52b23", "0xbcda"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_sign upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "unknown account"
  }
}

```

### Body Parameters

Here is the list of body parameters for `eth_sign` method:

1. **jsonrpc**: Specifies the version of the JSON-RPC protocol. Typically, this is "2.0".
2. **id**: A unique identifier for the request. It is used to match the response with the request.
3. **error**: An object that contains details about any error that occurred during the execution of the method.
   * **code**: A numeric code representing the error type. In this case, `-32000` indicates an "unknown account" error.
   * **message**: A brief description of the error. Here, it states "unknown account".

### Use Cases

Here are some use-cases for `eth_sign` method:

1. **Transaction Signing**: One of the primary uses of the `eth_sign` method is to sign transactions before they are sent to the Ethereum network. This ensures that the transaction is authorized by the owner of the account and has not been tampered with. By using `eth_sign`, developers can create secure transactions that are cryptographically signed, allowing them to be verified and executed on the blockchain.
2. **Message Authentication**: `eth_sign` can be used to sign arbitrary data, which is particularly useful for authenticating messages. In decentralized applications (dApps), users can sign messages to prove ownership of their Ethereum address. This is often used in login processes, where a user can sign a nonce provided by the dApp to authenticate themselves without revealing their private key.
3. **Smart Contract Interactions**: When interacting with smart contracts, `eth_sign` can be used to sign off-chain data that will be used in on-chain operations. This allows for complex interactions where data integrity and authenticity are crucial. For example, when submitting a bid in an auction contract, a user might sign their bid details off-chain, which are later verified on-chain for validity and authenticity.

### Code for eth\_sign

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
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "unknown account"
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
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "unknown account"
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

When using the `eth_sign` JSON-RPC API BSC method, the following issues may occur:

* **Invalid Address Format**: If the address provided is not in a valid hexadecimal format, the signing process will fail. Ensure the address is correctly formatted by checking for a proper "0x" prefix and a 40-character string.
* **Data Length Mismatch**: The data to be signed must be a valid hexadecimal string. If the length of the data is incorrect or not in hexadecimal format, the process will not proceed. Verify the data length and format before initiating the request.
* **Unauthorized Account Access**: If the account is locked or the private key is inaccessible, the signing will not be possible. Make sure the account is unlocked and the private key is correctly configured in your node setup.
* **Network Latency or Connectivity Issues**: Occasionally, network issues can lead to delayed or failed requests. Ensure stable network connectivity and consider implementing retries or timeout handling in your application.

The `eth_sign` method is invaluable in Web3 applications, allowing for the secure signing of messages with a specific account's private key. This functionality enables the creation of verifiable, tamper-proof transactions and messages, enhancing the security and authenticity of decentralized applications.

### Conclusion

The `eth_sign` method in JSON-RPC is a crucial function for signing messages with an Ethereum account's private key, ensuring the authenticity and integrity of transactions on networks like BSC. By leveraging `eth_sign`, developers can facilitate secure interactions and validate user intentions in decentralized applications.
