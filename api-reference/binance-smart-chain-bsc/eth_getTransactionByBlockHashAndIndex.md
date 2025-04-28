---
description: >-
  Access transaction details by block hash and index using
  eth_getTransactionByBlockHashAndIndex in the JSON-RPC API Interface for BSC.
---

# eth\_get TransactionByBlockHashAndIndex - Binance Smart Chain

{% hint style="success" %}
Retrieves transaction details from a specific block on Binance Smart Chain using block hash and transaction index.
{% endhint %}

The `eth_getTransactionByBlockHashAndIndex` method in the BSC protocol is an essential tool for retrieving transaction details by block hash and transaction index. As part of the `eth_getTransactionByBlockHashAndIndex` Web3 interface, this method facilitates precise access to transaction data, enhancing blockchain interaction.

Utilizing the `eth_getTransactionByBlockHashAndIndex` RPC protocol, developers can efficiently query specific transactions, enabling detailed analysis and data extraction. This method requires two parameters: the block hash and the transaction index, ensuring accurate and targeted data retrieval for seamless blockchain exploration.

### Supported Networks

The `eth_getTransactionByBlockHashAndIndex` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getTransactionByBlockHashAndIndex` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Block Hash**
  * **Type:** String
  * **Description:** The hash of the block containing the transaction.
  * **Required:** Yes
  * **Example Value:** `"0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f"`
* **Transaction Index**
  * **Type:** String
  * **Description:** The index position of the transaction within the block.
  * **Required:** Yes
  * **Example Value:** `"0x0"`

These parameters are necessary for the JSON-RPC request to obtain the transaction details from a specified block by its hash and the transaction's index within that block.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getTransactionByBlockHashAndIndex` :

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

**Response**

Below is a sample JSON response returned by `eth_getTransactionByBlockHashAndIndex` upon a successful call:

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

Here is the list of body parameters for `eth_getTransactionByBlockHashAndIndex` method:

1. **blockHash**: "0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f"
   * The hash of the block containing the transaction.
2. **blockNumber**: "0x52a96e"
   * The block number in which the transaction was included.
3. **from**: "0xb0fbdb31e382b7186d8d08c4a09c935a33a0996e"
   * The address of the sender.
4. **gas**: "0x3d090"
   * The gas provided by the sender.
5. **gasPrice**: "0x826299e00"
   * The price of gas for this transaction.
6. **hash**: "0x4847b1ebcbd04297a1d01c9724da0101d5fefd5f9d9f22068056231f0707f355"
   * The hash of the transaction.
7. **input**: "0xe2bbb15800000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000004f1a77ccd3ba0000"
   * The data sent along with the transaction.
8. **nonce**: "0x16"
   * The number of transactions made by the sender prior to this one.
9. **to**: "0x22cc57c9ec341152834f216289a1824d61b47855"
   * The address of the receiver. Null when it's a contract creation transaction.
10. **transactionIndex**: "0x0"
    * The index position of the transaction in the block.
11. **value**: "0x0"
    * The value transferred in Wei.
12. **type**: "0x0"
    * The transaction type.
13. **chainId**: "0x38"
    * The chain ID of the network.
14. **v**: "0x93"
    * The ECDSA recovery id.
15. **r**: "0xfc32ee2fee75718593b0f09b964c5cae54188b8d0556e525af1a811b501dd9b0"
    * The ECDSA signature r.
16. **s**: "0x589cd02a49468c49e48b2c1dd854026a9de7b87b0828af1ea7565c9148ee8377"
    * The ECDSA signature s.

### Use Cases

Here are some use-cases for `eth_getTransactionByBlockHashAndIndex` method:

1. **Transaction Verification**: In Web3 programming, verifying transactions is crucial for ensuring the integrity and authenticity of blockchain data. By using the `eth_getTransactionByBlockHashAndIndex` method, developers can retrieve a specific transaction from a block by providing the block's hash and the transaction's index within that block. This is particularly useful for applications that need to confirm whether a transaction has been successfully included in a specific block.
2. **Block Analysis**: For developers working on analytics tools or blockchain explorers, the `eth_getTransactionByBlockHashAndIndex` method allows for detailed examination of transactions within a block. By iterating over transaction indices, a program can compile a complete list of transactions in a given block, enabling analysis of transaction patterns, gas usage, or the identification of high-value transactions.
3. **Smart Contract Interaction**: When interacting with smart contracts, developers may need to track specific transactions to ensure that contract calls have been executed as expected. By using the `eth_getTransactionByBlockHashAndIndex` method, developers can pinpoint the exact transaction that corresponds to a smart contract interaction, allowing them to verify the transaction's details, such as input data and gas costs, and ensure that the contract behaved as intended.

### Code for eth\_getTransactionByBlockHashAndIndex

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
  "method": "eth_getTransactionByBlockHashAndIndex",
  "params": ["0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f", "0x0"],
  "id": 1
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
  "method": "eth_getTransactionByBlockHashAndIndex",
  "params": ["0x94c626286c237e187454ddd993adc534f0ce3468aaec134a39fbd52185cc3a5f", "0x0"],
  "id": 1
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

When using the `eth_getTransactionByBlockHashAndIndex` JSON-RPC API BSC method, the following issues may occur:

* Incorrect Block Hash: If the block hash provided is incorrect or does not exist, the method will return a null response. Ensure the block hash is valid by cross-referencing with a reliable block explorer.
* Index Out of Range: Providing an index that exceeds the number of transactions in the block will result in a null response. Verify the transaction count within the block to ensure the index is within range.
* Network Latency: High network latency can lead to delayed responses or timeouts. Consider implementing retry logic or increasing the timeout settings in your application to accommodate network variability.

Using the `eth_getTransactionByBlockHashAndIndex` method in Web3 applications allows developers to efficiently retrieve transaction details by specifying both the block hash and transaction index. This method is particularly beneficial for applications requiring precise transaction data retrieval within a specific block, enhancing the accuracy and performance of blockchain-based solutions.

### Conclusion

The `eth_getTransactionByBlockHashAndIndex` method is a JSON-RPC call that enables users to retrieve a specific transaction from a block using the block's hash and the transaction's index. This method is particularly useful for developers and users working with Ethereum-based blockchains, such as BSC (Binance Smart Chain), where precise transaction data is essential for various applications. By utilizing `eth_getTransactionByBlockHashAndIndex`, one can efficiently access transaction details, enhancing the ability to monitor and analyze blockchain activities.
