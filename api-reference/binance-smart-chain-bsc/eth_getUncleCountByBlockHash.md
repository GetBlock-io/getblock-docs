---
description: >-
  Use the JSON-RPC API Interface to access 'eth_getUncleCountByBlockHash' for retrieving uncle count by block hash in BSC.
---

# eth_getUncleCountByBlockHash

{% hint style="success" %}
The RPC method retrieves the number of uncles for a specific block identified by its hash on the Binance Smart Chain.&#x20;
{% endhint %}

The eth_getUncleCountByBlockHash Web3 method is a crucial part of the JSON-RPC API, specifically designed for developers working with the Binance Smart Chain (BSC). This method allows users to retrieve the number of uncles associated with a specific block, identified by its hash. As part of the eth_getUncleCountByBlockHash RPC protocol, it provides a streamlined way to gather data about a block's uncles, which are essentially orphaned blocks that are not part of the main chain but are still recognized by the network. By using this method, developers can efficiently query uncle data, which is vital for understanding block validation and network performance. This method is indispensable for applications that require detailed blockchain analytics and insights.

### Supported Networks

The eth_getUncleCountByBlockHash REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getUncleCountByBlockHash method needs to be executed:

- **Block Hash**:
  - **Required**: Yes
  - **Type**: String
  - **Description**: The hash of the block for which you want to retrieve the uncle count. This hash uniquely identifies a block in the blockchain.
  - **Supported Values**: A 32-byte hexadecimal string prefixed with "0x".

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getUncleCountByBlockHash

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getUncleCountByBlockHash",
  "params": ["0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f"],
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
  "result": "0x0"
}

```

### Body Parameters

Here is the list of body parameters for eth_getUncleCountByBlockHash method:

1. **jsonrpc**: The version of the JSON-RPC protocol. Typically, this is "2.0".
2. **id**: A unique identifier for the request. It is used to match the response with the request.
3. **result**: The result of the method call. For this method, it returns the number of uncles for the given block hash, represented as a hexadecimal string (e.g., "0x0").

### Use Cases

Here are some use-cases for eth_getUncleCountByBlockHash method:

1. **Blockchain Data Analysis**: Developers and researchers can use this method to analyze the frequency and distribution of uncle blocks in the Ethereum blockchain. By retrieving the number of uncles for specific blocks, they can gain insights into mining performance, network congestion, and the effectiveness of consensus mechanisms.

2. **Network Performance Monitoring**: Blockchain infrastructure providers and network operators can utilize this method to monitor the health and performance of the Ethereum network. By tracking the occurrence of uncle blocks, they can identify potential issues with block propagation and optimize their nodes for better efficiency and reliability.

3. **Historical Data Verification**: For applications that require historical data verification, such as auditing or forensic analysis, this method can be used to verify the integrity and completeness of blockchain data. By checking the uncle count for given blocks, developers can ensure that their data sources are accurate and trustworthy.

### Code for eth_getUncleCountByBlockHash

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
  "result": "0x0"
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
When using the eth_getUncleCountByBlockHash JSON-RPC API BSC method, the following issues may occur:  
- Invalid block hash: Ensure that the block hash provided is a valid 32-byte hexadecimal string. Double-check for typos or incorrect formatting in the hash input.  
- Non-existent block hash: The block hash might not exist on the blockchain if it's from a future block or a fork. Verify that the block hash corresponds to a confirmed block within the blockchain.  
- Network connectivity issues: If there is a failure in the network connection to the BSC node, the request might not be processed. Ensure stable network connectivity and verify that the node is operational.  
- Incorrect JSON-RPC version: Ensure that the JSON-RPC version specified is "2.0" as required by the protocol. Using an incorrect version might lead to compatibility issues.

Using the eth_getUncleCountByBlockHash method in Web3 applications provides a straightforward way to access the number of uncles associated with a specific block, which can be crucial for analyzing block efficiency and mining rewards. This functionality enables developers to gather comprehensive data on blockchain performance and contribute to more robust and data-driven applications.

### conclusion

The eth_getUncleCountByBlockHash JSON-RPC method is used to retrieve the number of uncles for a specific block identified by its hash on blockchain networks like Ethereum or Binance Smart Chain (BSC). This method is crucial for developers and analysts who need to understand block validation and uncle block occurrences in these networks. By utilizing eth_getUncleCountByBlockHash, one can gain insights into the network's performance and efficiency.
