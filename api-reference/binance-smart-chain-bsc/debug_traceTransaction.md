---
description: >-
  Explore transaction details with debug_traceTransaction via the JSON-RPC API
  Interface. Gain insights into execution traces on the BSC protocol.
---

# debug\_traceTransaction - Binance Smart Chain

{% hint style="success" %}
The RPC method provides detailed execution traces of a transaction on BSC, helping developers debug and analyze smart contract behavior and performance.
{% endhint %}

The `debug_traceTransaction` method in the BSC protocol is a powerful tool for developers and analysts, allowing them to trace the execution of a transaction. This method, part of the `debug_traceTransaction` Web3 suite, provides detailed insights into the state changes and operations executed during a transaction's lifecycle.

Utilizing the `debug_traceTransaction` RPC protocol, users can retrieve granular information such as stack traces and memory modifications. This aids in debugging smart contracts by offering a comprehensive view of the transaction's execution path, helping to identify issues and optimize performance.

### Supported Networks

The `debug_traceTransaction` JSON-RPC API method supports the following network types:

* **Mainnet**
* **Testnet**

### Parameters

Here is the list of parameters `debug_traceTransaction` method needs to be executed. Always format the method name as inline code (wrapped in backticks).

* **Parameter 1:**
  * **Type:** String
  * **Description:** The hash of the transaction to be traced.
  * **Required/Optional:** Required
  * **Default/Supported Values:** A valid transaction hash.
* **Parameter 2:**
  * **Type:** Object or Null
  * **Description:** Additional options for tracing. Passing `null` uses default settings.
  * **Required/Optional:** Optional
  * **Default/Supported Values:** `null` or an object with specific tracing options (e.g., `{"tracer": "callTracer", "timeout": "60s"}`).

## Request Example

**API Endpoint**

```json
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Request**

Here’s a sample cURL request using `debug_traceTransaction` :

{% tabs %}
{% tab title="curl" %}
```json
curl --location --request POST https://go.getblock.io/<ACCESS-TOKEN>/
--header 'Content-Type: application/json' 
--data-raw {"jsonrpc": "2.0",
"method": "debug_traceTransaction",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c", null],
"id": "getblock.io"}
```
{% endtab %}
{% endtabs %}

**Response**

Below is a sample JSON response returned by `debug_traceTransaction` upon a successful call:

```json

{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "error": {
    "code": -32000,
    "message": "transaction not found"
  }
}

```

### Body Parameters

Here is the list of body parameters for `debug_traceTransaction` method:

1. **jsonrpc**: The version of the JSON-RPC protocol being used. Typically, this is "2.0".
2. **id**: The identifier for the request, which helps in matching responses to requests. In this case, it is "getblock.io".
3. **error**: An object containing details about the error.
   * **code**: The error code, which in this example is -32000, indicating that the transaction was not found.
   * **message**: A descriptive message about the error. Here, it states "transaction not found".

### Use Cases

Here are some use-cases for `debug_traceTransaction` method:

1. **Transaction Debugging**: One of the primary use cases for the `debug_traceTransaction` method is to debug transactions on the Ethereum blockchain. Developers can use this method to obtain detailed execution traces of a transaction, which includes information about every operation executed, the state changes, and any errors that occurred. This is especially useful for identifying issues in smart contract execution, such as out-of-gas errors or unexpected behavior.
2. **Gas Usage Analysis**: Another important use case is analyzing the gas usage of a transaction. By tracing a transaction with `debug_traceTransaction`, developers can understand how much gas each operation consumed. This can help in optimizing smart contracts to reduce gas costs, which is crucial for cost-effective deployment and execution on the Ethereum network.
3. **Security Auditing**: Security auditors can utilize `debug_traceTransaction` to perform thorough audits of smart contracts. By examining the detailed execution trace, auditors can identify potential vulnerabilities, such as reentrancy attacks or unauthorized access to contract functions. This method provides a granular view of the transaction execution, making it an invaluable tool for ensuring the security and integrity of smart contracts.

### Code for debug\_traceTransaction

{% tabs %}
{% tab title="Python" %}
```python

import requests
import json
url = "https://go.getblock.io/<ACCESS-TOKEN>/"
headers = {
    "Content-Type": "application/json"
}
payload = {"jsonrpc": "2.0",
"method": "debug_traceTransaction",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c", null],
"id": "getblock.io"}

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
const payload = {"jsonrpc": "2.0",
"method": "debug_traceTransaction",
"params": ["0xcd718a69d478340dc28fdf6bf8056374a52dc95841b44083163ced8dfe29310c", null],
"id": "getblock.io"};

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

When using the `debug_traceTransaction` JSON-RPC API BSC method, the following issues may occur:

* **Missing Transaction Hash:** Ensure the transaction hash is correct and exists on the blockchain. Verify that the transaction has been mined and is accessible.
* **Node Configuration Issues:** If the node is not configured to support debug methods, the request will fail. Ensure the node is running with debug capabilities enabled.
* **Resource Limitations:** Tracing a transaction can be resource-intensive, potentially leading to timeouts. Consider increasing the node's computational resources or using a more powerful node provider.
* **Null or Incorrect Parameters:** Providing incorrect parameters, such as a malformed transaction hash, will result in errors. Double-check that all parameters are correctly formatted and valid.

Using the `debug_traceTransaction` method in Web3 applications provides developers with deep insights into transaction execution. By tracing transactions, developers can diagnose issues, optimize smart contracts, and enhance the reliability of their decentralized applications. This method is a powerful tool for debugging and improving blockchain-based systems.

### Conclusion

The `debug_traceTransaction` JSON-RPC method is a powerful tool for developers and analysts working with blockchain transactions on networks like BSC (Binance Smart Chain). By providing detailed execution traces, it allows for in-depth debugging and analysis of transaction behavior. This utility is essential for understanding complex transaction flows and diagnosing issues in smart contract interactions.
