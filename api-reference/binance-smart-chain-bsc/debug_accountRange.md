---
description: >-
  Explore debug_accountRange in the JSON-RPC API Interface for efficient
  blockchain account data retrieval on BSC.
---

# debug\_accountRange - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves a list of accounts within a specified range on the BSC, aiding in account management and analysis.
{% endhint %}

The `debug_accountRange` Web3 method is a specialized function within the BSC protocol, designed to facilitate the retrieval of blockchain account data over a specified range. As part of the debug\_accountRange RPC protocol, this method provides users with the ability to access detailed account information efficiently, aiding in debugging and data analysis tasks. By specifying parameters such as the start and end account addresses, users can tailor their queries to focus on specific segments of the blockchain, optimizing both time and resource usage. Ideal for developers and blockchain analysts, this method enhances the capability to monitor, debug, and understand on-chain activities within the Binance Smart Chain environment.

### Supported Networks

The debug\_accountRange JSON RPC API method supports the following network types

* **Mainnet**

### Parameters

Here is the list of parameters debug\_accountRange method needs to be executed:

* **Parameter 1**:
  * **Type**: String
  * **Description**: The root hash of the state trie.
  * **Required**: Yes
  * **Example Value**: "0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"
* **Parameter 2**:
  * **Type**: String
  * **Description**: The starting address or prefix from which to begin the account range query.
  * **Required**: Yes
  * **Example Value**: "0x0f"
* **Parameter 3**:
  * **Type**: Number
  * **Description**: The maximum number of results to return.
  * **Required**: Yes
  * **Example Value**: 0
* **Parameter 4**:
  * **Type**: Boolean
  * **Description**: Whether to include storage data in the result.
  * **Required**: Yes
  * **Supported Values**: true, false
  * **Default Value**: false
* **Parameter 5**:
  * **Type**: Boolean
  * **Description**: Whether to include code data in the result.
  * **Required**: Yes
  * **Supported Values**: true, false
  * **Default Value**: false
* **Parameter 6**:
  * **Type**: Boolean
  * **Description**: Whether to include account balance in the result.
  * **Required**: Yes
  * **Supported Values**: true, false
  * **Default Value**: false

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

Here’s a sample cURL request using debug\_accountRange

#### Request

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

### Response

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

### Body Parameters

Here is the list of body parameters for debug\_accountRange method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used, typically "2.0".
2. **id**: A unique identifier for the request, which can be used to match the response with the request.
3. **error**: An object containing error details if the request was unsuccessful.
   * **code**: A numeric code representing the error type.
   * **message**: A descriptive message providing more information about the error.

### Use Cases

Here are some use-cases for debug\_accountRange method in Web3 programming:

1. **Blockchain Auditing and Analysis**: This method can be used for auditing purposes, allowing developers and auditors to inspect a range of accounts within a specific block. By analyzing the state of accounts at a particular point in the blockchain, it's possible to track changes over time, detect anomalies, and ensure the integrity of the blockchain's data.
2. **Smart Contract Debugging**: Developers can utilize this method to debug smart contracts by examining the state of accounts that interact with a specific contract. By retrieving the account state before and after transactions, developers can identify issues, optimize contract performance, and ensure that the contract behaves as expected.
3. **Historical Data Retrieval**: For applications that require historical data analysis, this method can retrieve account states from past blocks. This is useful for generating reports, performing retrospective analyses, and understanding how account balances and states have evolved over time, which can be crucial for financial applications and decision-making processes.

### Code for debug\_accountRange

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
{% endtabs %}

## Common Errors

Common Errors\
When using the debug\_accountRange JSON-RPC API BSC method, the following issues may occur:

* Incorrect block hash: Ensure the block hash provided is valid and corresponds to an existing block. Double-check the hash format and value against the blockchain data.
* Invalid start key: The start key must be a valid hexadecimal string. Verify that the key is correctly formatted and within the range of the specified block.
* Out of range parameters: If the start or end parameters exceed the block's account range, adjust them to fit within the valid range. Confirm the block's account count to set appropriate boundaries.
* API call limitations: Excessive requests may lead to rate limiting by the node provider. Implement request throttling or batching to comply with usage policies.

Using the debug\_accountRange method in Web3 applications allows developers to efficiently retrieve account information within a specified range, facilitating detailed analysis and debugging. This capability enhances the ability to monitor account activities and optimize smart contract interactions, contributing to more robust and reliable DApps.

### conclusion

The debug\_accountRange JSON-RPC method is a valuable tool for developers working with the Binance Smart Chain (BSC), allowing them to efficiently query account ranges for debugging purposes. By providing parameters such as a block hash, starting index, and flags for inclusion of storage and code, this method facilitates a deeper understanding of account states and their transitions on the BSC. Integrating debug\_accountRange into development workflows can significantly enhance the debugging and analysis process on the blockchain.
