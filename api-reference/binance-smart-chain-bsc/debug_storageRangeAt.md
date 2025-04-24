---
description: >-
  Explore debug_storageRangeAt in the JSON-RPC API Interface for detailed storage insights on the BSC protocol.
---

# debug_storageRangeAt

{% hint style="success" %}
The RPC method retrieves a specified contract's storage range at a given block, aiding in debugging and analysis on the Binance Smart Chain.&#x20;
{% endhint %}

The debug_storageRangeAt Web3 method is a powerful tool within the BSC protocol, designed for developers and technical users seeking in-depth insights into smart contract storage. Utilizing the debug_storageRangeAt RPC protocol, this method allows users to query the storage of a specific contract at a given block, providing a snapshot of key-value pairs within a specified range. This can be particularly useful for debugging and analyzing contract behavior over time. By specifying parameters like the block hash, transaction index, and key range, users can efficiently pinpoint and examine storage data, facilitating a deeper understanding of contract states and interactions.

### Supported Networks

The debug_storageRangeAt REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters debug_storageRangeAt method needs to be executed:

- **Parameter 1**:  
  - **Type**: String  
  - **Description**: The block hash to inspect.  
  - **Required**: Yes  
  - **Default/Supported Values**: 32-byte hash string.

- **Parameter 2**:  
  - **Type**: Integer  
  - **Description**: The transaction index within the block.  
  - **Required**: Yes  
  - **Default/Supported Values**: Non-negative integer.

- **Parameter 3**:  
  - **Type**: String  
  - **Description**: The address of the contract to inspect.  
  - **Required**: Yes  
  - **Default/Supported Values**: 20-byte address string.

- **Parameter 4**:  
  - **Type**: String  
  - **Description**: The storage key to start searching from.  
  - **Required**: Yes  
  - **Default/Supported Values**: 32-byte hash string.

- **Parameter 5**:  
  - **Type**: Integer  
  - **Description**: The maximum number of storage entries to return.  
  - **Required**: Yes  
  - **Default/Supported Values**: Positive integer.

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using debug_storageRangeAt

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "debug_storageRangeAt",
"params": ["0x6e6a058ac35224a9842a8010042ddc93dea2f073260a8d79cd3368a232c3e7ed", 3, "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", "0xc94770007dda54cF92009BFF0dE90c06F603a09f", 1],
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
    "message": "historical state not available in path scheme yet"
  }
}

```

### Body Parameters

Here is the list of body parameters for debug_storageRangeAt method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used. Typically "2.0".
2. **id**: A unique identifier for the request, which can be a string or number. In this case, it is "getblock.io".
3. **error**: An object containing details about the error that occurred.
   - **code**: A numeric code representing the error type. Here, it is -32000.
   - **message**: A descriptive message providing more details about the error. In this response, the message is "historical state not available in path scheme yet".

### Use Cases

Here are some use-cases for debug_storageRangeAt method:

1. **Smart Contract Debugging**: This method is particularly useful for developers who need to debug smart contracts. By examining the storage of a contract at a specific block, developers can trace how the state of the contract has changed over time. This can help identify bugs or unexpected behavior in the contract logic by providing insights into the values stored at different stages of execution.

2. **Historical State Analysis**: In scenarios where developers or auditors need to understand the historical state of a smart contract, this method allows them to access the storage at a specific block number. This can be crucial for auditing purposes, as it helps verify the integrity and correctness of the contract's state at any given point in the past.

3. **Forensic Investigations**: In the case of security incidents or disputes, this method can be employed to perform forensic investigations. By examining the contract's storage at various block heights, investigators can reconstruct events leading up to an incident, such as unauthorized access or state manipulation, and gather evidence to support their findings.

### Code for debug_storageRangeAt

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
    "message": "historical state not available in path scheme yet"
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
When using the debug_storageRangeAt JSON-RPC API BSC method, the following issues may occur:  
- Incorrect Block Hash: If the block hash provided is incorrect or not found in the blockchain, the method will fail. Ensure that the block hash is accurate and corresponds to an existing block.  
- Invalid Account Address: Providing an invalid or incorrectly formatted account address can lead to errors. Double-check the address format and ensure it is a valid Ethereum address.  
- Out of Range Slot: Specifying a storage slot that is out of range or does not exist can result in an error. Verify that the storage slot index is within the valid range for the account's storage.  
- Network Latency: Slow network response or timeouts can occur if the node is under heavy load. Consider using a more powerful node or optimizing network configurations to improve performance.  

Using the debug_storageRangeAt method in Web3 applications allows developers to inspect and analyze smart contract storage at a specific block, providing valuable insights into contract state changes over time. This functionality is essential for debugging complex contracts and ensuring the integrity of decentralized applications. By leveraging this method, developers can enhance the reliability and performance of their blockchain solutions.

### conclusion

The debug_storageRangeAt JSON-RPC method is a powerful tool for developers working on the Binance Smart Chain (BSC), enabling them to inspect the storage state of a smart contract at a specific block and transaction. By providing detailed insights into storage slots, this method aids in debugging and optimizing smart contract performance on the BSC.
