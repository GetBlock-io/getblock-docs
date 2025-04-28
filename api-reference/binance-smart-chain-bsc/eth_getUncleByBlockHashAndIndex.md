---
description: >-
  Retrieve uncle block details by block hash and index using eth_getUncleByBlockHashAndIndex in the JSON-RPC API Interface for BSC protocol.
---

# eth_getUncleByBlockHashAndIndex

{% hint style="success" %}
The RPC method retrieves an uncle block from a specific Binance Smart Chain block using its hash and uncle index, aiding in blockchain analysis and validation.&#x20;
{% endhint %}

The `eth_getUncleByBlockHashAndIndex` method in the JSON-RPC API retrieves an uncle block from a specified block within the Binance Smart Chain (BSC) protocol. Utilizing the `eth_getUncleByBlockHashAndIndex Web3` interface, it requires the block hash and the uncle index as parameters, enabling precise access to uncle block data.

In the `eth_getUncleByBlockHashAndIndex RPC protocol`, this method is essential for developers needing detailed information about uncle blocks, which are alternative block proposals not included in the main chain. By providing a structured response, it facilitates efficient blockchain data analysis and validation.

## Supported Networks

The eth_getUncleByBlockHashAndIndex JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

Here is the list of parameters `eth_getUncleByBlockHashAndIndex` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

- **Block Hash**
  - **Type**: String
  - **Description**: The hash of the block containing the uncle.
  - **Required**: Yes
  - **Supported Values**: A 32-byte hexadecimal string, prefixed with "0x".

- **Uncle Index**
  - **Type**: String
  - **Description**: The index position of the uncle block in the block's list of uncles.
  - **Required**: Yes
  - **Supported Values**: A hexadecimal string representing the index, prefixed with "0x".

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using eth_getUncleByBlockHashAndIndex :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_getUncleByBlockHashAndIndex",
"params": ["0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f", "0x2"],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by eth_getUncleByBlockHashAndIndex upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": null
}

```

## Body Parameters

Here is the list of body parameters for the `eth_getUncleByBlockHashAndIndex` method:

1. **`jsonrpc`**: A string specifying the version of the JSON-RPC protocol. It is typically `"2.0"`.

2. **`id`**: A unique identifier for the request. It can be any string or number, and it is used to match the response with the request.

3. **`result`**: This is the main data field that contains the response to the method call. In this case, it is `null`, indicating that the method did not return a result (e.g., the uncle block was not found).

## Use Cases

Here are some use-cases for `eth_getUncleByBlockHashAndIndex` method:

1. **Blockchain Analysis and Research**: Developers and researchers can use the `eth_getUncleByBlockHashAndIndex` method to study the occurrence and characteristics of uncle blocks within the Ethereum network. By retrieving uncle blocks by their block hash and index, one can analyze patterns, understand the frequency of uncle block creation, and assess their impact on the blockchain's security and performance.

2. **Uncle Block Rewards Tracking**: Miners and mining pool operators can leverage this method to track uncle block rewards. Since uncle blocks are rewarded in Ethereum to incentivize miners for their efforts, accessing these blocks helps in calculating accurate mining rewards and understanding the profitability of mining operations.

3. **Network Health Monitoring**: By regularly querying uncle blocks using `eth_getUncleByBlockHashAndIndex`, network operators and developers can monitor the health and efficiency of the Ethereum network. A higher number of uncle blocks might indicate network congestion or latency issues, prompting further investigation and optimization efforts.

## Code for eth_getUncleByBlockHashAndIndex

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

## Common Errors

When using the `eth_getUncleByBlockHashAndIndex` JSON-RPC API BSC method, the following issues may occur:
- Incorrect block hash: If the provided block hash is invalid or does not exist, the method will return a null result. Ensure the block hash is correctly formatted and corresponds to a valid block on the BSC network.
- Index out of range: If the uncle index specified exceeds the number of uncles in the block, the method will return null. Verify the total number of uncles in the block and ensure the index is within this range.
- Network connectivity issues: Network disruptions or incorrect endpoint configurations can lead to failed requests. Double-check your network connection and confirm that the endpoint URL is correctly set to a functioning BSC node.

Using the `eth_getUncleByBlockHashAndIndex` method in Web3 applications provides developers with the ability to retrieve specific uncle blocks by their index in a given block. This functionality is crucial for analyzing blockchain data and understanding the structure of uncle blocks, which can aid in optimizing blockchain performance and security.

## Conclusion

The JSON-RPC method `eth_getUncleByBlockHashAndIndex` is used to retrieve information about an uncle block by specifying the block hash and the uncle's index. This method is essential for developers working with Ethereum or Binance Smart Chain (BSC) as it allows them to access detailed uncle block data efficiently. Understanding how to use `eth_getUncleByBlockHashAndIndex` is crucial for managing blockchain data and analyzing network performance.
