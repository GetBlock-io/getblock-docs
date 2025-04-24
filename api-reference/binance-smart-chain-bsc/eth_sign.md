---
description: >-
  Use eth_sign in the JSON-RPC API Interface to sign data with a specific account on the BSC protocol.
---

# eth_sign

{% hint style="success" %}
The RPC method allows users to sign messages with their private keys on the Binance Smart Chain, ensuring message authenticity and integrity without broadcasting a transaction.&#x20;
{% endhint %}

The eth_sign Web3 method in the BSC protocol allows users to sign arbitrary data with a specified account, leveraging the JSON-RPC API Interface. This functionality is crucial for verifying the origin of a message or transaction, ensuring that the data has not been tampered with. When using the eth_sign RPC protocol, a client sends a request containing the account address and the data to be signed. The response includes the corresponding signature, which can be used for authentication or validation purposes. This method is particularly useful for developers and applications that require secure message signing and verification processes on the Binance Smart Chain. It simplifies the process of generating cryptographic signatures, providing a seamless and efficient way to enhance security in decentralized applications.

### Supported Networks

The eth_sign REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_sign method needs to be executed.

- **Parameter 1**: 
  - **Type**: String
  - **Description**: The address of the account to sign with.
  - **Required**: Yes
  - **Default/Supported Values**: A valid Ethereum address in hexadecimal format (e.g., "0xcee8ae756461e2653b88aefdbd70c1144de52b23").

- **Parameter 2**: 
  - **Type**: String
  - **Description**: The data to sign.
  - **Required**: Yes
  - **Default/Supported Values**: A data payload in hexadecimal format (e.g., "0xbcda").

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_sign

#### Request

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

### Response


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

Here is the list of body parameters for eth_sign method:

1. `jsonrpc`: The version of the JSON-RPC protocol being used. Typically, this is "2.0".
2. `id`: An identifier for the request, which can be used to match responses with requests. In this case, it is "getblock.io".
3. `error`: Contains details about any error that occurred during the request.
   - `code`: A numerical code representing the specific error. Here, it is -32000.
   - `message`: A descriptive message providing more information about the error. In this instance, it is "unknown account".

### Use Cases

Here are some use-cases for eth_sign method in Web3 programming:

1. **Transaction Authorization**: This method can be used to authorize transactions on the Ethereum network. By signing a transaction with a private key, users can confirm their intent to send Ether or interact with a smart contract. This ensures that only the owner of the private key can initiate the transaction, providing a secure way to manage digital assets.

2. **Data Integrity Verification**: Developers can use this method to sign arbitrary data, which can then be used to verify the integrity and authenticity of the data. For example, a decentralized application (dApp) might require users to sign a message to prove ownership of a particular Ethereum address. This signed message can be verified by others to ensure that the data has not been tampered with and that it was indeed signed by the owner of the address.

3. **User Authentication**: In the context of dApps, this method can be employed for user authentication. Instead of traditional username and password systems, users can sign a unique message with their private key to log in. This approach leverages the security of cryptographic signatures, providing a seamless and secure way for users to authenticate themselves without the need for additional credentials.

### Code for eth_sign

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_sign JSON-RPC API BSC method, the following issues may occur:  
- Invalid address format: Ensure the address is a valid Ethereum address starting with '0x' and is 42 characters long. Double-check for typos or incorrect characters.  
- Malformed data parameter: The data to be signed must be a properly formatted hexadecimal string. Verify that the string starts with '0x' and contains only valid hex characters.  
- Unsupported account type: Some wallets or nodes may not support signing with certain account types, such as contract accounts. Use a standard externally owned account (EOA) for signing operations.  
- Network connectivity issues: Ensure your node is correctly connected to the BSC network and that there are no network disruptions. Check your node's logs for any connectivity errors.  

Using the eth_sign method in Web3 applications provides a powerful way to authenticate transactions and messages, ensuring data integrity and user verification. It allows developers to implement secure cryptographic operations, enhancing trust and functionality in decentralized applications.

### conclusion

The eth_sign method in JSON-RPC is a crucial tool for signing data with a specific Ethereum account, as demonstrated in the example using the address on the Binance Smart Chain (BSC). This functionality is essential for verifying transactions or messages securely, ensuring that only the owner of the private key can authorize actions on the blockchain.
