---
description: >-
  Access transaction details via eth_getTransactionByBlockHashAndIndex on the JSON-RPC API Interface for BSC protocol.
---

# eth_getTransactionByBlockHashAndIndex

{% hint style="success" %}
This RPC retrieves a transaction from the Binance Smart Chain using a block hash and transaction index, enabling detailed transaction data access.&#x20;
{% endhint %}

The eth_getTransactionByBlockHashAndIndex Web3 method retrieves transaction details from the Binance Smart Chain (BSC) by specifying the block hash and transaction index. Utilizing the eth_getTransactionByBlockHashAndIndex RPC protocol, developers can efficiently access transaction data within a block, facilitating blockchain data analysis and application development. This method requires two parameters: the block hash, identifying the specific block, and the transaction index, indicating the transaction's position within that block. The response includes transaction details such as hash, nonce, block hash, block number, sender and receiver addresses, and more. This function is integral for applications needing precise transaction data retrieval in a decentralized environment.

### Supported Networks

The eth_getTransactionByBlockHashAndIndex REST API method supports the following network types
- **Mainnet**
- **Testnets**

### Parameters

Here is the list of parameters eth_getTransactionByBlockHashAndIndex method needs to be executed:

- **Block Hash**:
  - **Type**: String
  - **Description**: The hash of the block containing the transaction.
  - **Required**: Yes
  - **Supported Values**: A 32-byte hash string (e.g., "0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f").

- **Transaction Index**:
  - **Type**: String
  - **Description**: The index position of the transaction within the block.
  - **Required**: Yes
  - **Supported Values**: A hexadecimal string representing the index (e.g., "0x0" for the first transaction in the block).

### Request Example

#### API Endpoint

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```
Here’s a sample cURL request using eth_getTransactionByBlockHashAndIndex

#### Request

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getTransactionByBlockHashAndIndex",
  "params": ["0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f", "0x0"],
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
  "result": {
    "blockHash": "0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f",
    "blockNumber": "0x52a96e",
    "from": "0xb0fbdb31e382b7186d8d08c4a09c935a33a0996e",
    "gas": "0x3d090",
    "gasPrice": "0x826299e00",
    "hash": "0x4847b1ebcbd04297a1d01c9724da0101d5fefd5f9d9f22068056231f0707f355",
    "input": "0xe2bbb15800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000004f1a77ccd3ba0000",
    "nonce": "0x16",
    "to": "0x22cc57c9ec341152834f216289a1824d61b47855",
    "transactionIndex": "0x0",
    "value": "0x0",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0xfc32ee2fee75718593b0f09b964c5cae54188b8d0556e525af1a811b501dd9b0",
    "s": "0x589cd02a49468c49e48b2c1dd854026a9de7b87b0828af1ea7565c9148ee8377"
  }
}

```

### Body Parameters

Here is the list of body parameters for eth_getTransactionByBlockHashAndIndex method:

1. blockHash: "0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f"
2. blockNumber: "0x52a96e"
3. from: "0xb0fbdb31e382b7186d8d08c4a09c935a33a0996e"
4. gas: "0x3d090"
5. gasPrice: "0x826299e00"
6. hash: "0x4847b1ebcbd04297a1d01c9724da0101d5fefd5f9d9f22068056231f0707f355"
7. input: "0xe2bbb15800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000004f1a77ccd3ba0000"
8. nonce: "0x16"
9. to: "0x22cc57c9ec341152834f216289a1824d61b47855"
10. transactionIndex: "0x0"
11. value: "0x0"
12. type: "0x0"
13. chainId: "0x38"
14. v: "0x93"
15. r: "0xfc32ee2fee75718593b0f09b964c5cae54188b8d0556e525af1a811b501dd9b0"
16. s: "0x589cd02a49468c49e48b2c1dd854026a9de7b87b0828af1ea7565c9148ee8377"

### Use Cases

Here are some use-cases for eth_getTransactionByBlockHashAndIndex method:

1. Transaction Verification: Developers can use this method to verify the details of a specific transaction within a block by providing the block hash and the transaction index. This is particularly useful for applications that need to confirm the integrity and authenticity of transactions, such as in financial services where transaction validation is critical.

2. Blockchain Analytics: Analysts and developers can leverage this method to extract specific transactions from a block for further analysis. This can be used to track transaction patterns, analyze gas usage, or study the interactions between smart contracts and users over time, which can provide insights into network performance and user behavior.

3. Debugging and Monitoring: In the development and maintenance of decentralized applications, this method can be utilized to monitor specific transactions within a block, helping developers identify issues or unexpected behavior in their applications. By retrieving transaction details, developers can debug errors, optimize gas usage, and ensure that their smart contracts are functioning as intended.

### Code for eth_getTransactionByBlockHashAndIndex

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
  "result": {
    "blockHash": "0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f",
    "blockNumber": "0x52a96e",
    "from": "0xb0fbdb31e382b7186d8d08c4a09c935a33a0996e",
    "gas": "0x3d090",
    "gasPrice": "0x826299e00",
    "hash": "0x4847b1ebcbd04297a1d01c9724da0101d5fefd5f9d9f22068056231f0707f355",
    "input": "0xe2bbb15800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000004f1a77ccd3ba0000",
    "nonce": "0x16",
    "to": "0x22cc57c9ec341152834f216289a1824d61b47855",
    "transactionIndex": "0x0",
    "value": "0x0",
    "type": "0x0",
    "chainId": "0x38",
    "v": "0x93",
    "r": "0xfc32ee2fee75718593b0f09b964c5cae54188b8d0556e525af1a811b501dd9b0",
    "s": "0x589cd02a49468c49e48b2c1dd854026a9de7b87b0828af1ea7565c9148ee8377"
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
When using the eth_getTransactionByBlockHashAndIndex JSON-RPC API BSC method, the following issues may occur:  
- Invalid block hash: Ensure that the block hash provided is a valid 32-byte hexadecimal string. Double-check for any typos or incorrect formatting.  
- Incorrect index value: The transaction index should be a hexadecimal string representing a non-negative integer. Verify that the index corresponds to a valid transaction within the block.  
- Block not found: If the specified block hash does not exist in the blockchain, you might be querying a non-existent block. Ensure the block has been mined and is part of the chain.  
- Network connectivity issues: If the node is unreachable or experiencing network issues, the request may fail. Check your network connection and the node's availability.

Using the eth_getTransactionByBlockHashAndIndex method in Web3 applications allows developers to retrieve specific transactions from a block efficiently. This capability is crucial for applications that require detailed transaction data for analytics, auditing, or real-time monitoring. By leveraging this method, developers can enhance the functionality and reliability of their blockchain-based solutions.

### conclusion

The eth_getTransactionByBlockHashAndIndex JSON-RPC method is a valuable tool for retrieving transaction details from a specific block on the Ethereum blockchain or Binance Smart Chain (BSC) using the block's hash and the transaction index. By leveraging this method, developers can efficiently access transaction data, which is crucial for applications that require precise blockchain information. As a part of the JSON-RPC API, it facilitates seamless interaction with blockchain networks like Ethereum and BSC, enhancing the capabilities of decentralized applications.
