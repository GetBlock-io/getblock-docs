---
description: >-
  Estimate gas for transactions using eth_estimateGas via the JSON-RPC API
  Interface on BSC, ensuring efficient gas usage and cost prediction.
---

# eth\_estimateGas - Binance Smart Chain

{% hint style="success" %}
The RPC method estimates the gas needed for a transaction on BSC, helping users set appropriate gas limits for successful execution.
{% endhint %}

The `eth_estimateGas` method in the BSC protocol is a crucial component for developers using the `eth_estimateGas` Web3 interface. This method estimates the gas required to execute a specific transaction, ensuring efficient gas usage by predicting the computational cost before execution.

Utilizing the `eth_estimateGas` RPC protocol, developers can simulate transactions to avoid failures due to insufficient gas. This method aids in optimizing transaction costs, enhancing the reliability and performance of smart contracts by providing accurate gas estimations.

### Supported Networks

The `eth_estimateGas` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `eth_estimateGas` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **`from`** (required)
  * **Type**: `string`
  * **Description**: The address the transaction is sent from.
  * **Default/Supported Values**: Must be a valid Ethereum address.
* **`to`** (required)
  * **Type**: `string`
  * **Description**: The address the transaction is directed to.
  * **Default/Supported Values**: Must be a valid Ethereum address.
* **`value`** (optional)
  * **Type**: `string`
  * **Description**: The amount of Ether to send with the transaction, specified in wei.
  * **Default/Supported Values**: Must be a hexadecimal string representing the amount in wei.

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `eth_estimateGas` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {
  "jsonrpc": "2.0",
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0xDd7Fd27CC153E303488e9cA7Ab0E38028Ac3eb35",
      "to": "0x1A0A18AC4BECDDbd6389559687d1A73d8927E416",
      "value": "0x186a0"
    }
  ],
  "id": "getblock.io"
}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by eth\_estimateGas upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x52fb"
}

```

### Body Parameters

Here is the list of body parameters for `eth_estimateGas` method:

1. **`jsonrpc`**: This parameter specifies the version of the JSON-RPC protocol being used. In this case, it is "2.0".
2. **`id`**: This is a unique identifier for the request, which can be used to match responses to requests. Here, it is set to "getblock.io".
3. **`result`**: This parameter contains the estimated gas amount required for the transaction. The value is returned in hexadecimal format. In this example, it is "0x52fb".

### Use Cases

Here are some use-cases for the `eth_estimateGas` method:

1. **Transaction Cost Estimation**: Before sending a transaction on the Ethereum network, developers can use the `eth_estimateGas` method to estimate the amount of gas required to execute the transaction. This helps in setting an appropriate gas limit, ensuring that the transaction has enough gas to be processed without being rejected due to insufficient gas.
2. **Smart Contract Interaction**: When interacting with smart contracts, especially complex ones, it's crucial to know how much gas will be consumed. The `eth_estimateGas` method allows developers to estimate the gas needed for calling a specific function within a smart contract. This ensures that the interaction is efficient and cost-effective.
3. **Optimizing Gas Usage**: Developers can use the `eth_estimateGas` method during the development and testing phases to optimize their smart contracts and transactions. By estimating gas usage for different scenarios, they can identify and refactor code that consumes excessive gas, leading to more efficient and cost-effective smart contract operations.

### Code for eth\_estimateGas

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
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0xDd7Fd27CC153E303488e9cA7Ab0E38028Ac3eb35",
      "to": "0x1A0A18AC4BECDDbd6389559687d1A73d8927E416",
      "value": "0x186a0"
    }
  ],
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
  "method": "eth_estimateGas",
  "params": [
    {
      "from": "0xDd7Fd27CC153E303488e9cA7Ab0E38028Ac3eb35",
      "to": "0x1A0A18AC4BECDDbd6389559687d1A73d8927E416",
      "value": "0x186a0"
    }
  ],
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

When using the `eth_estimateGas` JSON-RPC API BSC method, the following issues may occur:

* Insufficient funds error: This occurs when the `from` address does not have enough BNB to cover the estimated gas cost. Ensure that the account has sufficient balance before making the request.
* Invalid address format: If the `to` or `from` address is incorrectly formatted, the estimation will fail. Double-check that both addresses are valid and properly checksummed.
* Contract execution failure: If the transaction would fail when executed, `eth_estimateGas` might return an error. Verify the contract logic and input parameters to ensure they are correct.
* Network congestion: During periods of high network activity, the estimated gas might be inaccurate. Consider using a higher gas price to ensure the transaction is processed in a timely manner.

Using the `eth_estimateGas` method in Web3 applications provides a reliable way to predict the gas cost of a transaction before execution, allowing for more efficient gas management. By estimating gas, developers can optimize transaction costs and avoid failed transactions due to insufficient gas, enhancing the overall user experience in decentralized applications.

### Conclusion

The `eth_estimateGas` JSON-RPC method is a crucial tool for developers working on Ethereum and Binance Smart Chain (BSC) as it allows them to estimate the gas required for a transaction before execution. By providing an estimate, it helps in optimizing gas usage and ensures that the transaction has a higher likelihood of success without running out of gas. Using `eth_estimateGas` is an essential step in managing transaction costs effectively on Ethereum and BSC networks.
