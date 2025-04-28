---
description: >-
  The eth_call method in the JSON-RPC API Interface for BSC executes smart
  contract code without state changes, offering a read-only interaction.
---

# eth\_call - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves data from the blockchain without changing state, primarily used to read smart contract information on BSC.
{% endhint %}

The `eth_call` method in the BSC protocol is a crucial component of the `eth_call` Web3 API, allowing users to execute a new message call immediately without creating a transaction on the blockchain. This method is typically used to query data from smart contracts, providing a way to read contract state and execute code without any state change.

As part of the `eth_call` RPC protocol, this method requires parameters such as the transaction call object and the block number. The call object specifies details like the sender and recipient addresses, gas, and data payload. Since `eth_call` is executed locally on a node, it ensures fast and efficient querying without incurring gas fees, making it essential for developers interacting with smart contracts.

### Supported Networks

The eth\_call JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_call` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Parameter 1: Transaction Call Object**
  * **Type:** Object
  * **Description:** The transaction call object defines the message call to be made.
  * **Required:** Yes
  * **Fields:**
    * **to:**
      * **Type:** String
      * **Description:** The address of the contract to call.
      * **Required:** Yes
    * **data:**
      * **Type:** String
      * **Description:** The hash of the method signature and encoded parameters.
      * **Required:** Yes
* **Parameter 2: Block Number**
  * **Type:** String
  * **Description:** The block number to execute the call against.
  * **Required:** Yes
  * **Supported Values:**
    * `"latest"`: The latest mined block.
    * `"earliest"`: The earliest/genesis block.
    * `"pending"`: The pending state/transactions.
* **Parameter 3: State Override**
  * **Type:** Object
  * **Description:** Allows for temporary overriding of account state during the call.
  * **Required:** No
  * **Fields:**
    * **Address Key (e.g., `0x0fd43c8fabe26d70dfa4c8b6fa680db39f147460`):**
      * **Type:** Object
      * **Description:** Contains the state override for the specific address.
      * **Required:** No
      * **Sub-fields:**
        * **code:**
          * **Type:** String
          * **Description:** The EVM bytecode to be used instead of the existing code at the address.
          * **Required:** No

This breakdown provides a concise understanding of the parameters needed to execute the `eth_call` method.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_call :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_call",
  "params": [
    {
      "to": "0x0fd43c8fabe26d70dfa4c8b6fa680db39f147460",
      "data": "0x919840ad"
    },
    "latest",
    {
      "0x0fd43c8fabe26d70dfa4c8b6fa680db39f147460": {
        "code": "0x6080604052348015600f57600080fd5b506004361060285760003560e01c8063919840ad14602d575b600080fd5b60336045565b60408051918252519081900360200190f35b60005a90509056fea265627a7a72315820df124583906aafd283490b866399b6762e2075e1d84214363893c5993a13276f64736f6c63430005110032"
      }
    }
  ],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_call upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x00000000000000000000000000000000000000000000000000000000017d2582"
}

```

### Body Parameters

Here is the list of body parameters for `eth_call` method:

1. **jsonrpc**: The version of the JSON-RPC protocol. Typically, this is "2.0".
2. **id**: An identifier to match the response with the request. In this case, it is "getblock.io".
3. **result**: The result of the `eth_call` method, which is the returned data from the executed contract function. In this example, it is "0x00000000000000000000000000000000000000000000000000000000017d2582".

### Use Cases

Here are some use-cases for the `eth_call` method in Web3 programming:

1. **Reading Smart Contract Data**: The `eth_call` method is commonly used to read data from smart contracts without making any state changes. For instance, it can be used to fetch the balance of a token holder, retrieve the current state of a contract, or get the result of a view or pure function. This is useful for applications that need to display data from the blockchain without altering it.
2. **Simulating Transactions**: Another use-case for `eth_call` is to simulate a transaction before actually executing it on the blockchain. This allows developers to verify that a transaction will succeed and to check the output of a function call without incurring any gas costs. This is particularly useful for testing and debugging smart contract interactions.
3. **Fetching Contract Metadata**: Developers can use `eth_call` to fetch metadata from a smart contract, such as the contract's name, symbol, or any other details exposed by the contract's functions. This is essential for applications that need to display information about various tokens or smart contracts to users.

By using `eth_call`, developers can efficiently interact with the Ethereum blockchain to gather necessary data without modifying the blockchain state, ensuring cost-effective and safe operations.

### Code for eth\_call

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
  "result": "0x00000000000000000000000000000000000000000000000000000000017d2582"
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
  "result": "0x00000000000000000000000000000000000000000000000000000000017d2582"
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

When using the `eth_call` JSON-RPC API BSC method, the following issues may occur:

* Incorrect `data` encoding: If the `data` field is not properly encoded or matches the expected function signature, the call will fail. Ensure the function selector and parameters are correctly encoded in hexadecimal format.
* Invalid contract address: If the `to` address does not correspond to a deployed contract, the call will not execute. Verify that the contract address is correct and that the contract is deployed on the BSC network.
* Outdated block state: Using an outdated block reference for state can lead to inconsistent results. Always specify "latest" for the most recent block data or ensure the block number is accurate.
* Insufficient gas limit: If the gas limit is too low, the call might revert. Adjust the gas limit to accommodate the complexity of the function being called, even though `eth_call` is a read-only operation.

The `eth_call` method is invaluable in Web3 applications for simulating transactions and retrieving data from smart contracts without modifying the blockchain state. It allows developers to test and debug contract interactions efficiently, ensuring smooth deployment and operation of decentralized applications.

### Conclusion

The `eth_call` method in JSON-RPC is a powerful tool for reading data from smart contracts on blockchain networks like BSC (Binance Smart Chain) without making any state changes. By simulating a transaction, `eth_call` allows developers to query the blockchain efficiently, enabling the retrieval of information and testing of smart contract interactions.
