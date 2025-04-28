---
description: >-
  Access detailed storage state in BSC using debug_storageRangeAt via the
  JSON-RPC API Interface. Ideal for developers seeking precise blockchain data.
---

# debug\_storageRangeAt - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves a range of storage entries for a specific BSC contract, aiding in debugging by examining storage content at a particular block.
{% endhint %}

The `debug_storageRangeAt` method in the BSC protocol is a JSON-RPC API function that enables developers to inspect the storage state of a smart contract at a specific block. This method, part of the `debug_storageRangeAt` Web3 suite, allows users to fetch storage entries in a specified range, facilitating detailed analysis.

Utilizing the `debug_storageRangeAt` RPC protocol, developers can specify parameters such as block hash, transaction index, and contract address to retrieve storage data efficiently. This method is crucial for debugging and optimizing smart contracts, offering a granular view of storage variables and their values at given blockchain states.

### Supported Networks

The `debug_storageRangeAt` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `debug_storageRangeAt` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Block Hash** (Required)
  * **Type**: String
  * **Description**: The hash of the block from which to retrieve the storage range.
  * **Default/Supported Values**: Must be a valid block hash in hexadecimal format.
* **Transaction Index** (Required)
  * **Type**: Integer
  * **Description**: The index of the transaction within the block for which to retrieve the storage range.
  * **Default/Supported Values**: Must be a non-negative integer.
* **Contract Address** (Required)
  * **Type**: String
  * **Description**: The address of the contract whose storage is being queried.
  * **Default/Supported Values**: Must be a valid Ethereum address in hexadecimal format.
* **Start Key** (Required)
  * **Type**: String
  * **Description**: The starting key for the storage range query.
  * **Default/Supported Values**: Must be a valid storage key in hexadecimal format.
* **Max Results** (Required)
  * **Type**: Integer
  * **Description**: The maximum number of key-value pairs to return.
  * **Default/Supported Values**: Must be a positive integer.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `debug_storageRangeAt` :

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

**Response**

Below is a sample JSON response returned by `debug_storageRangeAt` upon a successful call:

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

Here is the list of body parameters for `debug_storageRangeAt` method:

1. **jsonrpc**: Version of the JSON-RPC protocol being used. Typically, this is "2.0".
2. **id**: An identifier for the request, which can be used to match the response with the request.
3. **error**: An object containing error details if the request fails. It includes:
   * **code**: A numeric code representing the error type. In this case, `-32000` indicates that the historical state is not available.
   * **message**: A descriptive message explaining the error. Here, it states "historical state not available in path scheme yet".

### Use Cases

Here are some use-cases for `debug_storageRangeAt` method:

1. **Smart Contract Debugging**: The `debug_storageRangeAt` method is invaluable for developers who need to debug smart contracts. By allowing access to the storage of a contract at a specific block, developers can inspect the state of the contract at any given point in time. This can help in identifying bugs or unexpected behavior by comparing the expected storage state with the actual one.
2. **Historical Data Analysis**: For developers and analysts interested in understanding how the state of a smart contract has evolved over time, `debug_storageRangeAt` provides a way to retrieve historical storage data. This can be particularly useful for auditing purposes or for analyzing patterns in how a contract's state changes in response to different transactions.
3. **Security Audits**: During security audits, it is crucial to verify that a smart contract's state transitions are secure and as intended. `debug_storageRangeAt` allows auditors to examine the storage state at different points in time, helping them to identify potential vulnerabilities or deviations from expected behavior in the contract's execution.

### Code for debug\_storageRangeAt

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0",
"method": "debug_storageRangeAt",
"params": ["0x6e6a058ac35224a9842a8010042ddc93dea2f073260a8d79cd3368a232c3e7ed", 3, "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", "0xc94770007dda54cF92009BFF0dE90c06F603a09f", 1],
"id": "getblock.io"}

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
const payload = {"jsonrpc": "2.0",
"method": "debug_storageRangeAt",
"params": ["0x6e6a058ac35224a9842a8010042ddc93dea2f073260a8d79cd3368a232c3e7ed", 3, "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", "0xc94770007dda54cF92009BFF0dE90c06F603a09f", 1],
"id": "getblock.io"};

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

When using the `debug_storageRangeAt` JSON-RPC API BSC method, the following issues may occur:

* Incorrect block hash: If the block hash provided is incorrect or does not exist, the method will fail to retrieve the storage range. Ensure the block hash is accurate and corresponds to a valid block number.
* Invalid contract address: Providing an incorrect contract address can lead to no storage data being returned. Double-check the contract address to ensure it is valid and deployed on the BSC network.
* Out of range storage key: If the storage key specified is out of the range of the contract's storage, the method will not return any data. Verify the storage key and ensure it is within the contract's storage range.
* Network connectivity issues: Poor network connectivity can result in timeouts or failed requests. Ensure a stable and reliable internet connection when making the request.

Utilizing the `debug_storageRangeAt` method in Web3 applications provides developers with a powerful tool for inspecting the storage state of smart contracts at specific blocks. This capability is invaluable for debugging and understanding contract behavior over time, helping developers optimize and troubleshoot their decentralized applications effectively.

### Conclusion

The JSON-RPC method `debug_storageRangeAt` is used to inspect the storage of a smart contract at a specific block on blockchain networks like Ethereum or Binance Smart Chain (BSC). By providing parameters such as the block hash, transaction index, and contract address, users can retrieve detailed storage information for debugging purposes. This makes `debug_storageRangeAt` a valuable tool for developers working on BSC to identify and resolve issues within smart contracts.
