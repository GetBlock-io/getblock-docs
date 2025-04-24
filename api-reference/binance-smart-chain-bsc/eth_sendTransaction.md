---
description: >-
  Use eth_sendTransaction in the JSON-RPC API Interface to send transactions on the BSC network efficiently.
---

# eth_sendTransaction

{% hint style="success" %}
The RPC method sends a transaction on the Binance Smart Chain, facilitating token transfers or contract interactions by specifying transaction details like recipient, value, and data.&#x20;
{% endhint %}

The eth_sendTransaction Web3 method is a critical function within the Binance Smart Chain (BSC) ecosystem, enabling users to send transactions seamlessly through the eth_sendTransaction RPC protocol. This method facilitates the transfer of funds or execution of smart contracts by specifying parameters such as the recipient address, value, and optional data payloads. When invoked, it generates a transaction that is signed and broadcast to the network. Users can customize gas price and limit to optimize transaction costs. As part of the JSON-RPC API Interface, this method requires a connection to a BSC node, ensuring secure and reliable transaction processing. Ideal for developers and users seeking efficient blockchain interactions, it supports various use cases from simple transfers to complex decentralized applications.

### Supported Networks

The eth_sendTransaction REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_sendTransaction method needs to be executed.

- **from** (required)
  - **Type:** string
  - **Description:** The address of the sender.
  - **Default/Supported Values:** A valid Ethereum address.

- **to** (required)
  - **Type:** string
  - **Description:** The address of the recipient.
  - **Default/Supported Values:** A valid Ethereum address.

- **gas** (optional)
  - **Type:** string
  - **Description:** The gas limit provided for the transaction.
  - **Default/Supported Values:** Hexadecimal string representing the gas limit. Default depends on the network or client settings.

- **gasPrice** (optional)
  - **Type:** string
  - **Description:** The price per unit of gas in wei.
  - **Default/Supported Values:** Hexadecimal string representing the gas price. Default depends on the network or client settings.

- **value** (optional)
  - **Type:** string
  - **Description:** The amount of ether to send with the transaction.
  - **Default/Supported Values:** Hexadecimal string representing the value in wei.

- **data** (optional)
  - **Type:** string
  - **Description:** The compiled code of a contract OR the hash of the invoked method signature and encoded parameters.
  - **Default/Supported Values:** Hexadecimal string representing the data payload.

- **id** (optional)
  - **Type:** string
  - **Description:** An identifier established by the client to track the request.
  - **Default/Supported Values:** Any string value, used to match responses with requests.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_sendTransaction

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_sendTransaction",
"params": [{"from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155", "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567", "gas": "0x76c0", "gasPrice": "0x9184e72a000", "value": "0x9184e72a", "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"}],
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

Here is the list of body parameters for eth_sendTransaction method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used. In this context, it is "2.0".

2. **id**: A unique identifier for the request. This helps in matching responses with requests in asynchronous environments. In this example, it is "getblock.io".

3. **error**: An object containing details about any error that occurred during the execution of the method. This includes:
   - **code**: A numeric error code. In this case, it is -32000, which indicates an error related to the method execution.
   - **message**: A human-readable message providing more details about the error. Here, it is "unknown account", indicating that the account specified in the transaction is not recognized.

### Use Cases

Here are some use-cases for eth_sendTransaction method in Web3 programming:

1. **Transferring Ether**: One of the primary use cases is to transfer Ether from one account to another. This is a fundamental operation on the Ethereum network, allowing users to send cryptocurrency to peers, pay for services, or make donations. By specifying the sender's and recipient's addresses, the amount of Ether to send, and the gas price, developers can facilitate these transactions programmatically.

2. **Interacting with Smart Contracts**: Developers often use this method to interact with deployed smart contracts. By including the contract's address in the 'to' field and encoding the function call in the 'data' field, users can invoke contract functions, pass parameters, and modify the contract's state. This is essential for decentralized applications that require interactions beyond simple Ether transfers.

3. **Deploying Smart Contracts**: Another use case is deploying new smart contracts to the Ethereum blockchain. By omitting the 'to' field and including the contract's bytecode in the 'data' field, developers can use this method to create new contracts. This process involves sending a transaction that includes the compiled code, which the Ethereum network then processes to instantiate the contract on the blockchain.

### Code for eth_sendTransaction

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
When using the eth_sendTransaction JSON-RPC API BSC method, the following issues may occur:  
- Insufficient gas: If the gas limit is set too low, the transaction may fail due to insufficient gas. Ensure that the gas limit is adequate for the transaction complexity and network conditions.  
- Incorrect nonce: Using an incorrect nonce can cause transaction failures or delays. Verify that the nonce is correctly incremented for each transaction from the same account.  
- Invalid recipient address: Sending a transaction to an invalid or incorrectly formatted address will result in failure. Double-check the recipient address to ensure it is valid and correctly formatted.  
- Insufficient funds: If the sender's account balance is lower than the transaction value plus gas fees, the transaction will not be processed. Verify that the sender's account has sufficient funds to cover the transaction and associated costs.  

Using the eth_sendTransaction method in Web3 applications allows for seamless interaction with the blockchain, enabling users to send transactions directly from their applications. This method is essential for executing smart contracts and transferring assets, providing a robust foundation for decentralized applications.

### conclusion

The eth_sendTransaction method in the JSON-RPC protocol is crucial for initiating transactions on Ethereum and Binance Smart Chain (BSC) networks. It allows users to send Ether or tokens by specifying parameters such as the sender and recipient addresses, gas, gas price, and transaction data. This method facilitates seamless interaction with smart contracts and decentralized applications on these blockchain platforms.
