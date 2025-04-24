---
description: >-
  Use eth_sendRawTransaction in the JSON-RPC API Interface to broadcast signed transactions on the BSC network seamlessly.
---

# eth_sendRawTransaction

{% hint style="success" %}
The RPC method eth_sendRawTransaction on BSC broadcasts a pre-signed transaction for network validation and inclusion in the blockchain.&#x20;
{% endhint %}

The eth_sendRawTransaction Web3 method is a core component of the Binance Smart Chain (BSC) network, allowing users to broadcast signed transactions directly through the eth_sendRawTransaction RPC protocol. This method requires the transaction to be pre-signed, ensuring that the sender's private key remains secure and never leaves their environment. By accepting a serialized, hex-encoded transaction, it facilitates efficient and secure transaction processing. Users can leverage this method to interact with smart contracts, transfer tokens, or execute other blockchain operations without exposing sensitive information. The eth_sendRawTransaction Web3 method is essential for developers and users seeking to perform transactions programmatically, offering a streamlined and secure approach within the BSC ecosystem.

### Supported Networks

The eth_sendRawTransaction REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_sendRawTransaction method needs to be executed:

- **Parameter**: signed transaction
  - **Required/Optional**: Required
  - **Type**: String
  - **Description**: The raw signed transaction data that you want to send to the Ethereum network. This should be encoded as a hexadecimal string.
  - **Default/Supported Values**: The value must be a valid signed transaction in hexadecimal format.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_sendRawTransaction

#### Request

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

### Response


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

Here is the list of body parameters for eth_sendRawTransaction method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used. Typically, this is "2.0".

2. **id**: A unique identifier for the request. This is usually a number, but it can also be a string. It's used to match responses with requests.

3. **error**: An object containing details about any errors that occurred during the processing of the request. This includes:
   - **code**: A numeric error code indicating the type of error.
   - **message**: A descriptive message providing more details about the error.

Note: The error message in your example indicates that the transaction data provided is not properly formatted. It should be a hex string prefixed with "0x".

### Use Cases

Here are some use-cases for eth_sendRawTransaction method in Web3 programming:

1. **Broadcasting Transactions**: This method is commonly used to broadcast a pre-signed transaction to the Ethereum network. Developers can sign transactions offline using a private key and then send the raw transaction data to the network, which ensures that the private key does not need to be exposed to the network or the client application, enhancing security.

2. **Automating Transactions**: In scenarios where transactions need to be automated, such as in decentralized applications (dApps) or smart contract interactions, eth_sendRawTransaction can be used to programmatically send transactions without manual intervention. This is particularly useful for executing a series of transactions or interacting with smart contracts in a seamless and efficient manner.

3. **Handling Complex Transactions**: For users or applications that need to handle complex transactions involving multiple steps or conditions, sending raw transactions allows for greater control over the transaction parameters. Developers can customize gas prices, nonce values, and other transaction details before signing and sending them to the network, which can optimize transaction processing and cost.

### Code for eth_sendRawTransaction

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_sendRawTransaction JSON-RPC API BSC method, the following issues may occur:  
- Invalid Signature: If the transaction signature is incorrect, the network will reject it. Ensure the private key used for signing matches the sender's address.  
- Insufficient Gas: Transactions with gas limits set too low will not be processed. Verify that your gas limit covers the computational cost of the transaction.  
- Nonce Mismatch: A mismatch in the transaction nonce can lead to failures. Check and synchronize the nonce with the current transaction count of the sender's address.  
- Insufficient Funds: If the sender's account lacks the necessary balance to cover the transaction value and gas fees, the transaction will fail. Ensure the account has adequate funds before sending.  

Using the eth_sendRawTransaction method allows Web3 applications to broadcast pre-signed transactions directly to the network, enhancing security by keeping private keys off the server. This method enables more efficient and flexible transaction handling, empowering developers to create seamless user experiences in decentralized applications.

### conclusion

The eth_sendRawTransaction method in JSON-RPC is a crucial tool for broadcasting signed transactions to the Ethereum network or Binance Smart Chain (BSC). By using this method, developers can directly interact with the blockchain, ensuring secure and efficient transaction processing. Understanding how to implement eth_sendRawTransaction within these networks is essential for leveraging the full potential of decentralized applications.
