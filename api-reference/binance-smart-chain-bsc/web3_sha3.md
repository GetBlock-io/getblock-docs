---
description: >-
  web3_sha3: A JSON-RPC API Interface method in BSC protocol for computing Keccak-256 hash of input data, offering secure and efficient hashing.
---

# web3_sha3

{% hint style="success" %}
The RPC method computes the Keccak-256 hash of the given input data, used for data integrity and verification on the Binance Smart Chain.&#x20;
{% endhint %}

The `web3_sha3` method in the BSC protocol is a JSON-RPC API function that computes the Keccak-256 hash of the given input data. As part of the `web3_sha3 Web3` suite, this method is essential for developers needing to generate cryptographic hashes, ensuring data integrity and security within decentralized applications.

Utilizing the `web3_sha3 RPC protocol`, developers can send a JSON-RPC request with a single parameter, the input data in hexadecimal format, to receive the corresponding Keccak-256 hash. This method is crucial for tasks such as verifying data authenticity and is a fundamental tool in blockchain development.

## Supported Networks

The web3_sha3 JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

Here is the list of parameters `web3_sha3` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

- **Parameter**: `data`
  - **Type**: String
  - **Description**: The data to be hashed using the Keccak-256 algorithm. This should be provided as a hexadecimal string prefixed with "0x".
  - **Required**: Yes
  - **Default/Supported Values**: Any valid hexadecimal string representing the data to hash. In this request, the provided value is `"0x68656c6c6f20776f726c64"`, which is the hexadecimal representation of the ASCII string "hello world".

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using web3_sha3 :

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

#### Response

Below is a sample JSON response returned by web3_sha3 upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}

```

## Body Parameters

Here is the list of body parameters for the `web3_sha3` method:

1. **jsonrpc**: The version of the JSON-RPC protocol used, which is typically "2.0".
2. **id**: A unique identifier for the request, used to match the response with the request. In this case, it is "1".
3. **result**: The resulting hexadecimal hash string computed by the `web3_sha3` method. In this example, it is "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad".

## Use Cases

Here are some use-cases for `web3_sha3` method:

1. **Data Integrity Verification**: The `web3_sha3` method is often used to ensure data integrity by generating a hash of the data. This is particularly useful in blockchain applications where you need to verify that data has not been altered. By hashing the original data and comparing it to the stored hash, developers can confirm that the data remains unchanged.

2. **Creating Unique Identifiers**: In decentralized applications (dApps), it's common to need unique identifiers for transactions, blocks, or user data. By using `web3_sha3`, developers can generate a unique hash for any given input, ensuring that identifiers are both unique and consistent.

3. **Smart Contract Development**: When developing smart contracts, `web3_sha3` can be used to hash data before storing it on the blockchain. This is particularly useful for storing sensitive information, as the hash can be stored instead of the raw data, providing an additional layer of security. Additionally, hashing is used for creating cryptographic proofs, such as in the implementation of Merkle trees, which are fundamental in efficient data verification processes.

## Code for web3_sha3

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
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const url = "https://go.getblock.io/<ACCESS-TOKEN>/";
const payload = {
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
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

## Common Errors

When using the `web3_sha3` JSON-RPC API BSC method, the following issues may occur:
- Incorrect input format: The input data must be in hexadecimal format prefixed with "0x". Ensure that the input string is properly formatted to prevent errors.
- Data length mismatch: The input data length should be an even number of characters. Double-check that the input string's length is correct and adjust if necessary.
- Network connectivity issues: If the BSC node is unreachable, the hash calculation will fail. Verify your network connection and ensure the node endpoint is correct and accessible.
- Unsupported characters: The input data should only contain valid hexadecimal characters (0-9, a-f). Validate your input to remove any invalid characters that could cause the method to fail.

Using the `web3_sha3` method in Web3 applications provides a reliable way to compute the Keccak-256 hash of any input data, which is essential for tasks such as verifying data integrity and creating unique identifiers. This method is fundamental for developers to ensure data consistency and security within decentralized applications on the BSC network.

## Conclusion

The `web3_sha3` method in JSON-RPC is a crucial tool for hashing data, ensuring data integrity and security in blockchain environments like BSC. By converting input data into a fixed-size hash, `web3_sha3` helps developers verify data authenticity, making it a foundational component in the BSC ecosystem.
