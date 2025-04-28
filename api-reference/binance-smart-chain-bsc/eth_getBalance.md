---
description: >-
  Retrieve account balance using eth_getBalance via the JSON-RPC API Interface
  on the BSC protocol, offering precise and efficient data access.
---

# eth\_getBalance - Binance Smart Chain

{% hint style="success" %}
The RPC method retrieves an account's balance in wei on the Binance Smart Chain, providing the current balance at a specified block number.
{% endhint %}

The `eth_getBalance` method in the BSC protocol is an essential JSON-RPC API call that retrieves the balance of a specified address in wei. This method is commonly used in the `eth_getBalance` Web3 context to provide developers with precise account balance information at a specific block height, enhancing blockchain interactions.

Utilizing the `eth_getBalance` RPC protocol, users can specify the target Ethereum address and the block parameter, which can be a block number or keywords like `latest`, `earliest`, or `pending`. This flexibility allows developers to obtain real-time or historical balance data, facilitating accurate financial assessments in decentralized applications.

### Supported Networks

The `eth_getBalance` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_getBalance` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Address**
  * **Type**: String
  * **Description**: The address of the account whose balance is being queried.
  * **Required**: Yes
  * **Default/Supported Values**: Must be a valid Ethereum address, typically starting with "0x".
* **Block Parameter**
  * **Type**: String
  * **Description**: The block number, or one of the predefined block parameters ("latest", "earliest", "pending").
  * **Required**: Yes
  * **Default/Supported Values**: "latest", "earliest", "pending", or a specific block number in hexadecimal format.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_getBalance` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_getBalance",
  "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `eth_getBalance` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x0"
}

```

### Body Parameters

Here is the list of body parameters for the `eth_getBalance` method:

1. **`jsonrpc`**: The version of the JSON-RPC protocol. Typically, it is "2.0".
2. **`id`**: A unique identifier for the request. In this example, it is "getblock.io".
3. **`result`**: The balance of the account in hexadecimal format. In this example, it is "0x0", which represents zero balance.

These parameters are part of the response when querying the balance of an Ethereum account using the `eth_getBalance` method.

### Use Cases

Here are some use-cases for `eth_getBalance` method:

1. **Wallet Balance Display**: One of the primary use-cases for the `eth_getBalance` method is to display the current balance of an Ethereum address in a wallet application. By calling this method, a wallet app can fetch the latest balance of a user's address and display it in the user interface, allowing users to track their Ether holdings in real-time.
2. **Transaction Validation**: Before initiating a transaction, developers can use `eth_getBalance` to ensure that an Ethereum address has sufficient funds to cover the transaction amount and associated gas fees. This pre-check helps in preventing failed transactions due to insufficient balance, thereby improving the user experience and transaction success rate.
3. **Portfolio Management**: In portfolio management applications, `eth_getBalance` can be used to aggregate and display the total value of an individual's Ethereum holdings across multiple addresses. By retrieving the balance of each address, the application can calculate the total Ether owned and provide insights into the user's overall portfolio value.

### Code for eth\_getBalance

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
  "method": "eth_getBalance",
  "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
  "id": "getblock.io"
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
  "method": "eth_getBalance",
  "params": ["0x8D97689C9818892B700e27F316cc3E41e17fBeb9", "latest"],
  "id": "getblock.io"
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

When using the `eth_getBalance` JSON-RPC API BSC method, the following issues may occur:

* Incorrect Address Format: If the Ethereum address is not properly formatted, the request will fail. Ensure the address is a valid 42-character hexadecimal string starting with '0x'.
* Network Synchronization Lag: Sometimes, the BSC node may not be fully synced with the network, leading to outdated balance information. Verify node synchronization status or switch to a different, up-to-date node.
* API Rate Limits: Excessive requests might trigger rate limits on the node provider, causing delayed or blocked responses. Consider implementing request throttling or using a dedicated node service with higher rate limits.
* Block Parameter Errors: Using an incorrect block parameter (e.g., non-existent block number) can lead to errors. Always use valid block identifiers like `"latest"` or specific block numbers.

The `eth_getBalance` method is invaluable in Web3 applications, providing real-time balance checks for Ethereum addresses on the BSC network. This functionality is crucial for developing responsive and user-friendly applications that require accurate financial data, enhancing user experience and application reliability.

### Conclusion

The `eth_getBalance` JSON-RPC method is a crucial tool for retrieving the balance of a specific Ethereum or BSC account at a given block. By using `eth_getBalance`, developers can efficiently monitor account balances, which is essential for managing transactions and smart contracts on the blockchain.
