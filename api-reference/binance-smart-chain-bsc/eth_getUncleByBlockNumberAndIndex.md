---
description: >-
  Access uncle block details with eth_getUncleByBlockNumberAndIndex using the JSON-RPC API Interface on the BSC protocol.
---

# eth_getUncleByBlockNumberAndIndex

{% hint style="success" %}
The RPC method retrieves information about an uncle block by block number and index on the Binance Smart Chain, aiding in blockchain analysis and validation.&#x20;
{% endhint %}

The eth_getUncleByBlockNumberAndIndex Web3 method allows users to retrieve details of an uncle block by specifying a block number and index. This functionality is part of the eth_getUncleByBlockNumberAndIndex RPC protocol, which serves as a vital tool for developers working with the Binance Smart Chain (BSC). By providing the block number and the index of the uncle, users can access specific information about uncle blocks, which are blocks that were mined simultaneously but not included in the main blockchain. This method is crucial for understanding the dynamics of block validation and network performance. Its integration into the JSON-RPC API Interface ensures seamless interaction with the BSC network, enabling efficient data retrieval in decentralized applications.

### Supported Networks

The eth_getUncleByBlockNumberAndIndex REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getUncleByBlockNumberAndIndex method needs to be executed.

- **Parameter 1: Block Number**
  - **Type:** String
  - **Description:** The block number from which the uncle block is to be retrieved. It is represented in hexadecimal format.
  - **Required:** Yes
  - **Default/Supported Values:** A valid block number in hexadecimal format, prefixed with "0x".

- **Parameter 2: Uncle Index**
  - **Type:** String
  - **Description:** The index position of the uncle block within the specified block. It is represented in hexadecimal format.
  - **Required:** Yes
  - **Default/Supported Values:** A valid index in hexadecimal format, prefixed with "0x".

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getUncleByBlockNumberAndIndex

#### Request

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

### Response


```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": null
}

```

### Body Parameters

Here is the list of body parameters for eth_getUncleByBlockNumberAndIndex method:

1. **jsonrpc**: The version of the JSON-RPC protocol. It should be "2.0".
2. **id**: An identifier for the request. It can be any string or number, often used to match requests and responses.
3. **result**: The result of the request. In this case, it is `null`, indicating that no uncle block was found for the specified block number and index.

### Use Cases

Here are some use-cases for eth_getUncleByBlockNumberAndIndex method:

1. **Blockchain Data Analysis**: Developers and analysts can use this method to gather detailed information about uncle blocks within Ethereum. By retrieving uncles based on block number and index, they can analyze the frequency and conditions under which uncle blocks occur. This information can be crucial for understanding network performance, block propagation times, and the overall efficiency of the Ethereum network.

2. **Uncle Block Rewards Verification**: In Ethereum, miners receive rewards for both regular blocks and uncle blocks. This method allows developers to verify the existence and details of uncle blocks associated with a particular block. By doing so, they can ensure that miners are accurately compensated for their contributions to the network, thereby maintaining the integrity and fairness of reward distribution.

3. **Historical Data Retrieval**: For applications that require historical blockchain data, such as blockchain explorers or analytics platforms, this method provides a way to access uncle block information from past blocks. This can be valuable for reconstructing the state of the blockchain at a specific point in time, conducting audits, or providing users with comprehensive historical insights into blockchain activity.

### Code for eth_getUncleByBlockNumberAndIndex

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
{% endtabs %}

## Common Errors

Common Errors  
When using the eth_getUncleByBlockNumberAndIndex JSON-RPC API BSC method, the following issues may occur:  
- Incorrect block number format: Ensure the block number is provided in hexadecimal format, prefixed with "0x". Double-check your input to prevent conversion errors.  
- Index out of range: If the specified uncle index does not exist for the block, verify the block's uncle count to ensure the index is valid.  
- Network connectivity issues: A failure to connect to the BSC node may result in timeouts or no response. Verify your network connection and node status to ensure proper communication.  
- Invalid parameters: Incorrectly formatted parameters can lead to errors. Ensure both parameters are correctly formatted and valid to avoid issues.

Using the eth_getUncleByBlockNumberAndIndex method in Web3 applications provides valuable insights into the blockchain's structure by retrieving uncle blocks. This can be useful for analyzing block propagation and orphaned block occurrences, enhancing the understanding and optimization of blockchain performance.

### conclusion

The eth_getUncleByBlockNumberAndIndex JSON-RPC method is used to retrieve information about an uncle block by specifying the block number and index on the Ethereum blockchain. This function is crucial for developers working on Ethereum-based projects, including those on Binance Smart Chain (BSC), as it helps in analyzing block data and understanding blockchain dynamics. By leveraging eth_getUncleByBlockNumberAndIndex, developers can gain insights into the network's operation and performance.
