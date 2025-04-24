---
description: >-
  The web3_sha3 JSON-RPC API Interface computes Keccak-256 hash values, essential for secure data handling in blockchain applications.
---

# web3_sha3

{% hint style="success" %}
The RPC method computes the Keccak-256 hash of given input data, primarily used for verifying data integrity and authenticity on the Binance Smart Chain.&#x20;
{% endhint %}

The web3_sha3 Web3 method is a crucial component of the BSC protocol, designed to compute the Keccak-256 hash of a given input. As part of the JSON-RPC API interface, this method ensures data integrity and security by generating a cryptographic hash, which is vital in various blockchain operations. When you call the web3_sha3 RPC protocol, you provide a string input, and the method returns a 32-byte hash. This functionality is pivotal for developers dealing with Ethereum-compatible environments, as it allows for consistent and secure data handling across decentralized applications. With its straightforward implementation, web3_sha3 is an indispensable tool for achieving robust data security in blockchain technology.

### Supported Networks

The web3_sha3 REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters web3_sha3 method needs to be executed.

- **Parameter**: Data
  - **Type**: String
  - **Description**: The data to be hashed using the Keccak-256 algorithm. It should be provided as a hexadecimal string prefixed with "0x".
  - **Requirement**: Required
  - **Default/Supported Values**: Hexadecimal string representing the data to hash.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using web3_sha3

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "web3_sha3",
  "params": ["0x68656c6c6f20776f726c64"],
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
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}

```

### Body Parameters

Here is the list of body parameters for web3_sha3 method:

1. `jsonrpc`: This parameter specifies the version of the JSON-RPC protocol being used. In this case, it is "2.0".

2. `id`: This parameter is a unique identifier for the request. It helps in matching responses to their corresponding requests. Here, the value is 1.

3. `result`: This parameter contains the result of the web3_sha3 method call. It is a hash value, represented as a hexadecimal string. In this example, the result is "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad".

### Use Cases

Here are some use-cases for web3_sha3 method:

1. **Data Integrity and Verification**: One of the primary uses of the web3_sha3 method is to ensure data integrity and verify data authenticity. By generating a hash of a given input, developers can compare it against a known hash to confirm that the data has not been altered. This is particularly useful in scenarios where data is transmitted over a network or stored in a decentralized manner, ensuring that any tampering can be easily detected.

2. **Smart Contract Development**: In the realm of smart contract development, this method can be used to create unique identifiers or keys. For instance, when storing records or transactions on the blockchain, hashing the input data can produce a unique key that represents the data without exposing the actual content. This is beneficial for maintaining privacy and ensuring that sensitive information is not directly stored on the blockchain.

3. **Digital Signatures and Authentication**: The web3_sha3 method plays a critical role in creating digital signatures and authenticating users or transactions. By hashing a message before signing it with a private key, developers can ensure that the signature is both unique to the message and verifiable by others using the corresponding public key. This process is essential for validating the authenticity of transactions and communications in blockchain applications.

### Code for web3_sha3

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
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
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
When using the web3_sha3 JSON-RPC API BSC method, the following issues may occur:  
- Incorrect parameter format: Ensure the input string is a valid hexadecimal format prefixed with '0x'. Double-check the input for any typos or missing characters.  
- Network connectivity issues: If there is a delay or lack of response, verify your network connection and ensure your node is properly synced with the Binance Smart Chain.  
- Unsupported characters in input: The input should only contain hexadecimal characters. Avoid using non-hexadecimal characters to prevent errors.  
- API version mismatch: Make sure you are using a compatible version of the API that supports the web3_sha3 method, as outdated versions may not function correctly.

Using the web3_sha3 method in Web3 applications provides a reliable way to generate hashes of input data, essential for data integrity and verification processes. This method enhances security by ensuring that data has not been tampered with, making it a fundamental tool for blockchain-based applications.

### conclusion

The web3_sha3 JSON-RPC method is a crucial function for developers working with blockchain platforms like Ethereum and Binance Smart Chain (BSC). It allows for the computation of the Keccak-256 hash of the given input data, which is essential for ensuring data integrity and security in decentralized applications. By leveraging web3_sha3, developers can efficiently handle cryptographic operations necessary for smart contract interactions on BSC and other Ethereum-compatible networks.
