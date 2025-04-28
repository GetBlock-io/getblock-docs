---
description: >-
  Explore account data efficiently with debug_accountRange using the JSON-RPC API Interface in the BSC protocol. Technical and user-friendly access.
---

# debug_accountRange

{% hint style="success" %}
The method retrieves account data within a specified range, aiding in blockchain analysis and monitoring on the Binance Smart Chain.&#x20;
{% endhint %}

The `debug_accountRange` method in the BSC protocol is a part of the `debug_accountRange Web3` suite, providing developers with the ability to retrieve detailed information on accounts within a specified range. This method is instrumental for debugging and analyzing account data, offering insights into account states and storage.

As an integral component of the `debug_accountRange RPC protocol`, this method facilitates efficient data retrieval through JSON-RPC, enhancing the debugging process. By specifying parameters like block number and account range, users can gain a comprehensive view of account details, aiding in the effective diagnosis of network behavior and performance issues.

## Supported Networks

The debug_accountRange JSON-RPC API method supports the following network types:
- **Mainnet**
- **Testnet**

## Parameters

Here is the list of parameters `debug_accountRange` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

- **Parameter 1:**
  - **Type:** String
  - **Description:** The root of the account trie.
  - **Required:** Yes
  - **Example Value:** `"0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"`

- **Parameter 2:**
  - **Type:** String
  - **Description:** The starting account address, represented as a hexadecimal string.
  - **Required:** Yes
  - **Example Value:** `"0x0f"`

- **Parameter 3:**
  - **Type:** Integer
  - **Description:** The maximum number of accounts to return.
  - **Required:** Yes
  - **Example Value:** `0`

- **Parameter 4:**
  - **Type:** Boolean
  - **Description:** Whether to include storage information.
  - **Required:** Yes
  - **Supported Values:** `true` or `false`
  - **Example Value:** `false`

- **Parameter 5:**
  - **Type:** Boolean
  - **Description:** Whether to include code information.
  - **Required:** Yes
  - **Supported Values:** `true` or `false`
  - **Example Value:** `false`

- **Parameter 6:**
  - **Type:** Boolean
  - **Description:** Whether to include account balance information.
  - **Required:** Yes
  - **Supported Values:** `true` or `false`
  - **Example Value:** `false`

# Request Example

##### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```


#### Request

Here’s a sample cURL request using debug_accountRange :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "debug_accountRange",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7", "0x0f", 0, false, false, false],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

#### Response

Below is a sample JSON response returned by debug_accountRange upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "block 0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7 not found"
  }
}

```

## Body Parameters

Here is the list of body parameters for the `debug_accountRange` method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used. Typically set to "2.0".
2. **id**: A unique identifier for the request, which can be used to match responses with requests.
3. **error**: An object containing details about any errors encountered during the execution of the method.
   - **code**: A numeric error code indicating the type of error that occurred.
   - **message**: A descriptive message providing more information about the error.

## Use Cases

Here are some use-cases for the `debug_accountRange` method:

1. **Blockchain Forensics and Analysis**: The `debug_accountRange` method can be used in blockchain forensics to analyze account activities within a specified range of blocks. This is particularly useful for identifying patterns of transactions or tracking the movement of funds across accounts, which can aid in detecting fraudulent activities or understanding the flow of assets in the blockchain network.

2. **Smart Contract Auditing**: Developers and auditors can use `debug_accountRange` to verify the behavior of smart contracts over a series of blocks. By examining the interactions of a contract with various accounts, auditors can ensure that the contract functions as intended and does not exhibit any unexpected or malicious behavior over time.

3. **Historical Data Retrieval**: For developers building analytics platforms or dashboards, `debug_accountRange` can be employed to retrieve historical account data efficiently. This can be beneficial for generating reports or visualizations that require an understanding of how account balances or activities have evolved over specific block ranges, providing insights into user behavior or network usage trends.

## Code for debug_accountRange

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
    "message": "block 0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7 not found"
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
    "message": "block 0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7 not found"
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

## Common Errors

When using the `debug_accountRange` JSON-RPC API BSC method, the following issues may occur:
- Incorrect block hash: If the block hash provided is invalid or does not exist, the method will fail. Ensure the block hash is accurate and corresponds to a valid block in the BSC network.
- Invalid range parameters: Supplying an incorrect range, such as a negative or excessively large range, can lead to errors. Verify that the range is within acceptable limits and corresponds to existing accounts.
- Network synchronization issues: If the node is not fully synced with the BSC network, the method may return incomplete or outdated data. Confirm that your node is fully synchronized before making the request.

Using the `debug_accountRange` method in Web3 applications can significantly enhance debugging capabilities by allowing developers to efficiently examine account states within a specific block range. This method provides valuable insights into contract interactions and account balances, facilitating the troubleshooting and optimization of decentralized applications on the BSC network.

## Conclusion

The `debug_accountRange` JSON-RPC method is a diagnostic tool used in blockchain environments like Binance Smart Chain (BSC) to retrieve information about accounts within a specified range. By using parameters such as the block hash, starting character, and additional flags, developers can effectively debug and analyze account data. This method is particularly useful for developers seeking to optimize performance and troubleshoot issues on BSC.
