---
description: >-
  eth_sendTransaction in the JSON-RPC API Interface allows you to create and
  send transactions on the BSC protocol efficiently and securely.
---

# eth\_sendTransaction - Binance Smart Chain

{% hint style="success" %}
The RPC method sends a transaction to the BSC network, facilitating actions like transferring tokens or executing smart contracts.
{% endhint %}

The `eth_sendTransaction` method in the BSC protocol allows users to create and send a transaction to the network. As part of the `eth_sendTransaction` Web3 interface, it facilitates interactions with smart contracts and token transfers. Users must specify transaction parameters like `from`, `to`, `value`, `gas`, and `data`.

Utilizing the `eth_sendTransaction` RPC protocol, this method requires a running Ethereum client to process requests. It is essential for developers to ensure the sending account is unlocked and has sufficient funds for the transaction. This method returns a transaction hash, which can be used to track the transaction status on the blockchain.

### Supported Networks

The eth\_sendTransaction JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_sendTransaction` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **`from`** (required)
  * **Type**: String
  * **Description**: The address of the sender.
  * **Default/Supported Values**: Must be a valid Ethereum address.
* **`to`** (required)
  * **Type**: String
  * **Description**: The address of the recipient.
  * **Default/Supported Values**: Must be a valid Ethereum address.
* **`gas`** (optional)
  * **Type**: String
  * **Description**: The amount of gas to use for the transaction.
  * **Default/Supported Values**: Hexadecimal format. If not specified, a default value is used based on network conditions.
* **`gasPrice`** (optional)
  * **Type**: String
  * **Description**: The price per unit of gas.
  * **Default/Supported Values**: Hexadecimal format. If not specified, the network's current gas price is used.
* **`value`** (optional)
  * **Type**: String
  * **Description**: The amount of Ether to send with the transaction.
  * **Default/Supported Values**: Hexadecimal format. Represents the amount in Wei.
* **`data`** (optional)
  * **Type**: String
  * **Description**: The compiled code of a contract or the hash of the invoked method signature and encoded parameters.
  * **Default/Supported Values**: Hexadecimal format. Used for contract interactions.
* **`id`** (optional)
  * **Type**: String
  * **Description**: An identifier for the request, which can be used to match responses to requests.
  * **Default/Supported Values**: Any string value, often used for tracking requests.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using eth\_sendTransaction :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "eth_sendTransaction",
"params": [{"from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155", "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567", "gas": "0x76c0", "gasPrice": "0x9184e72a000", "value": "0x9184e72a", "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"}],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_sendTransaction upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "unknown account"
  }
}

```

### Body Parameters

Here is the list of body parameters for `eth_sendTransaction` method:

1. **from**: The address of the sender.
2. **to**: The address of the receiver. This can be `null` if you are creating a contract.
3. **gas**: The gas provided for the transaction execution. It will return unused gas.
4. **gasPrice**: The price per unit of gas.
5. **value**: The value to be transferred in wei.
6. **data**: The compiled code of a contract OR the hash of the invoked method signature and encoded parameters.
7. **nonce**: The number of transactions made by the sender prior to this one.

### Use Cases

Here are some use-cases for `eth_sendTransaction` method:

1. **Transferring Ether Between Accounts**: One of the primary use-cases for the `eth_sendTransaction` method is to transfer Ether from one Ethereum account to another. This is a fundamental operation in Ethereum, allowing users to send Ether as a form of payment or transfer funds to another account address. The transaction includes details such as the sender's address, recipient's address, the amount of Ether to transfer, and optional parameters like gas price and gas limit.
2. **Interacting with Smart Contracts**: Another common use-case for the `eth_sendTransaction` method is to interact with smart contracts deployed on the Ethereum blockchain. By including the appropriate data payload in the transaction, developers can call functions on the smart contract, passing necessary parameters and invoking contract logic. This enables a wide range of decentralized applications (dApps) to perform operations such as token transfers, contract state updates, or triggering specific contract functionalities.
3. **Deploying Smart Contracts**: The `eth_sendTransaction` method can also be used to deploy new smart contracts to the Ethereum blockchain. By sending a transaction with the contract's compiled bytecode as the data payload and specifying the deploying account as the sender, developers can publish new contracts. This is an essential step in the lifecycle of dApps, allowing developers to introduce new functionalities and services to the Ethereum network.

### Code for eth\_sendTransaction

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
    "message": "unknown account"
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
    "message": "unknown account"
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

### Common Errors

When using the `eth_sendTransaction` JSON-RPC API BSC method, the following issues may occur:

* Insufficient Gas: If the gas limit is set too low, the transaction may fail. Ensure the gas limit is sufficient by estimating the gas required for the transaction using `eth_estimateGas`.
* Invalid Nonce: A transaction might be rejected if the nonce is incorrect. Verify that the nonce is set correctly by checking the latest nonce for the sender account with `eth_getTransactionCount`.
* Incorrect Gas Price: Setting a gas price too low can result in the transaction taking a long time to be included in a block. Adjust the gas price according to current network conditions using gas price estimation tools.
* Insufficient Funds: If the sender's account balance is lower than the transaction value plus gas costs, the transaction will fail. Ensure the sender's account has enough BNB to cover both the transaction value and the associated fees.

Using the `eth_sendTransaction` method in Web3 applications allows developers to programmatically send transactions, facilitating interactions with smart contracts and transferring tokens. This method is integral for creating dynamic, decentralized applications, enabling seamless, automated transactions on the blockchain.

### Conclusion

The `eth_sendTransaction` method is a crucial part of the JSON-RPC API, allowing for the transfer of Ether or execution of smart contracts on Ethereum and compatible networks like BSC (Binance Smart Chain). This method facilitates seamless interaction with the blockchain, enabling users to initiate transactions by specifying parameters such as sender, receiver, gas, and value. By leveraging `eth_sendTransaction`, developers can efficiently build decentralized applications that interact with Ethereum-based ecosystems.
