---
description: >-
  Access uncle block details by block number and index using
  eth_getUncleByBlockNumberAndIndex in the JSON-RPC API Interface for BSC.
---

# eth\_getUncleByBlockNumberAndIndex - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves an uncle block from a specific block number and index on the Binance Smart Chain, aiding in blockchain analysis and validation.
{% endhint %}

The `eth_getUncleByBlockNumberAndIndex` method in the BSC protocol is a JSON-RPC API call that retrieves information about an uncle block by specifying the block number and the uncle's index position. This method is essential for developers using the `eth_getUncleByBlockNumberAndIndex Web3` interface to access uncle block data efficiently.

Utilizing the `eth_getUncleByBlockNumberAndIndex RPC protocol`, developers can obtain details such as the uncle's hash, miner, and other metadata. This method is crucial for applications needing to analyze uncle blocks within the Binance Smart Chain, offering a straightforward way to fetch specific uncle data by block number and index.

### Supported Networks

The eth\_getUncleByBlockNumberAndIndex JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getUncleByBlockNumberAndIndex` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Parameter 1: Block Number**
  * **Type**: String
  * **Description**: The block number from which the uncle block is to be retrieved, represented as a hexadecimal string.
  * **Required**: Yes
  * **Default/Supported Values**: Must be a valid block number in hexadecimal format, prefixed with "0x".
* **Parameter 2: Uncle Index**
  * **Type**: String
  * **Description**: The index position of the uncle block within the specified block, represented as a hexadecimal string.
  * **Required**: Yes
  * **Default/Supported Values**: Must be a valid index in hexadecimal format, prefixed with "0x".

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_getUncleByBlockNumberAndIndex :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_getUncleByBlockNumberAndIndex",
"params": ["0x89D2", "0x0"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_getUncleByBlockNumberAndIndex upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": null
}

```

### Body Parameters

Here is the list of body parameters for the `eth_getUncleByBlockNumberAndIndex` method:

1. **jsonrpc**: This parameter specifies the version of the JSON-RPC protocol. It is typically set to `"2.0"`.
2. **id**: This parameter is a unique identifier for the request. It is used to match the response with the request. In this example, it is set to `"getblock.io"`.
3. **result**: This parameter contains the result of the method call. For the `eth_getUncleByBlockNumberAndIndex` method, it would include the details of the uncle block if the request is successful. In this case, it is `null`, indicating that no uncle block was found or there was an error in retrieving the data.

### Use Cases

Here are some use-cases for `eth_getUncleByBlockNumberAndIndex` method:

1. **Blockchain Analysis and Research**: Developers and researchers can use the `eth_getUncleByBlockNumberAndIndex` method to retrieve uncle blocks for a specific block number and index. This can be useful for analyzing the frequency and distribution of uncle blocks within the Ethereum blockchain, which can provide insights into network performance and mining efficiency.
2. **Mining Pool Monitoring**: Mining pools can utilize the `eth_getUncleByBlockNumberAndIndex` method to monitor the occurrence of uncle blocks that their miners produce. By tracking these uncles, pools can optimize their strategies to reduce the occurrence of uncle blocks, which generally result in lower rewards compared to successfully mined blocks.
3. **Reward Calculation**: In Ethereum, uncle blocks still receive a partial reward. Developers building tools for miners or mining pools can use the `eth_getUncleByBlockNumberAndIndex` method to accurately calculate the rewards associated with uncle blocks. This ensures that miners receive the correct compensation for their contributions to the network, even when their blocks are not included in the main chain.

### Code for eth\_getUncleByBlockNumberAndIndex

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
  "result": null
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
  "result": null
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

When using the `eth_getUncleByBlockNumberAndIndex` JSON-RPC API BSC method, the following issues may occur:

* Incorrect block number format: Ensure that the block number is provided in hexadecimal format prefixed with "0x". Double-check the conversion from decimal to hexadecimal to avoid this error.
* Index out of range: If the specified uncle index exceeds the number of uncles in the block, the method will return a null value. Verify the number of uncles in the target block to ensure the index is valid.
* Network connectivity issues: If the connection to the BSC node is unstable, the request may fail. Ensure that the network connection is reliable and that the node is responsive.
* Insufficient node permissions: Some nodes may restrict access to certain methods. Confirm that the node you are querying allows access to the `eth_getUncleByBlockNumberAndIndex` method.

Using the `eth_getUncleByBlockNumberAndIndex` method in Web3 applications provides valuable insights into the uncle blocks associated with a specific block, which can be crucial for understanding the block's consensus and mining rewards. This method allows developers to efficiently retrieve uncle block data, enhancing the ability to analyze blockchain performance and integrity.

### Conclusion

The `eth_getUncleByBlockNumberAndIndex` method is a JSON-RPC call used in Ethereum and compatible blockchains like Binance Smart Chain (BSC) to retrieve information about an uncle block by specifying the block number and the index of the uncle. This method is particularly useful for developers and analysts looking to understand the structure and rewards of uncle blocks within a blockchain network. By utilizing `eth_getUncleByBlockNumberAndIndex`, one can gain insights into the network's block validation process and the occurrence of uncle blocks.
